-- =====================================================================================================
--                          Bloco 1: Configuração Inicial e Variáveis Globais
-- =====================================================================================================

-- Determina em qual "mar" (mundo) do Blox Fruits o jogador está.
-- O 'PlaceId' é um identificador único para cada lugar (jogo ou mapa) no Roblox.
-- Blox Fruits tem PlaceIds diferentes para o Primeiro, Segundo e Terceiro Mar.
local World1 = false
local World2 = false
local World3 = false

if game.PlaceId == 2753915549 then -- PlaceId do Primeiro Mar
    World1 = true
    print("Detectado: Primeiro Mar")
elseif game.PlaceId == 4442272183 then -- PlaceId do Segundo Mar
    World2 = true
    print("Detectado: Segundo Mar")
elseif game.PlaceId == 7449423635 then -- PlaceId do Terceiro Mar
    World3 = true
    print("Detectado: Terceiro Mar")
else
    game:GetService("Players").LocalPlayer:Kick("Local não suportado. Por favor, entre em um dos mares suportados (Blox Fruits).")
end

-- Variáveis Globais (_G): Estas variáveis são acessíveis de qualquer parte do script.
-- Elas funcionam como "flags" para controlar o comportamento do auto-farm.
_G.Remove_Effect = true  -- Se for 'true', o script tentará remover o efeito de "morte" (tela escura/cinza).

_G.CurrentFarmActive = false -- Flag para saber se alguma função de farm está ativa.
_G.FarmType = nil            -- Armazena o tipo de farm ativo (ex: "BanditFarm", "GorillaFarm").

_G.AutoQuest = true          -- Controla se o script deve automaticamente pegar e entregar quests.
_G.AutoAttack = true         -- Controla se o script deve atacar os monstros.
_G.TargetMob = nil           -- Uma variável para armazenar uma referência ao monstro que está sendo alvo no momento.

local autoFarmThread = nil -- Variável para manter a referência à thread de farm, para poder pará-la.


-- =====================================================================================================
--                            Bloco 2: Funções Auxiliares Essenciais
-- =====================================================================================================

function Hop()
    print("[Hop] Tentando trocar de servidor...")
    local TeleportService = game:GetService("TeleportService")
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer

    if LocalPlayer then
        TeleportService:Teleport(game.PlaceId, LocalPlayer)
    else
        warn("[Hop] LocalPlayer não encontrado, não é possível teletransportar para outro servidor.")
    end
end

function Teleport(cframe)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    if LocalPlayer and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = cframe
        print("[Teleport] Teletransportado para: X=" .. math.floor(cframe.Position.X) .. ", Y=" .. math.floor(cframe.Position.Y) .. ", Z=" .. math.floor(cframe.Position.Z))
    else
        warn("[Teleport] HumanoidRootPart do jogador não encontrada para teletransporte. Personagem pode não estar carregado.")
    end
end

function FindMob(name)
    local workspace = game:GetService("Workspace")
    local foundMob = nil
    local shortestDistance = math.huge
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    if not PlayerHRP then
        return nil
    end

    for i, v in pairs(workspace:GetChildren()) do
        if v:IsA("Model") and
           v.Name:find(name, 1, true) and
           v:FindFirstChild("Humanoid") and
           v.Humanoid.Health > 0 and
           v:FindFirstChild("HumanoidRootPart") then

            local mobHRP = v.HumanoidRootPart
            local distance = (PlayerHRP.Position - mobHRP.Position).magnitude

            if distance < shortestDistance then
                shortestDistance = distance
                foundMob = v
            end
        end
    end
    return foundMob
end

function AttackTarget(target)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    if not (PlayerHRP and target and target:FindFirstChild("HumanoidRootPart") and target:FindFirstChild("Humanoid") and target.Humanoid.Health > 0) then
        return
    end

    PlayerHRP.CFrame = CFrame.new(PlayerHRP.Position, target.HumanoidRootPart.Position)

    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local AttackRemote = nil

    -- TENTATIVAS DE ENCONTRAR O REMOTE EVENT DE ATAQUE
    AttackRemote = ReplicatedStorage:FindFirstChild("Remote")
    if AttackRemote and AttackRemote:IsA("RemoteEvent") then
        AttackRemote:FireServer("Attack", target.HumanoidRootPart)
        -- print("[Attack] Tentando atacar via RemoteEvent 'Remote' com 'Attack' e alvo: " .. target.Name)
        wait(0.2) return
    end

    AttackRemote = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Attack")
    if AttackRemote and AttackRemote:IsA("RemoteEvent") then
        AttackRemote:FireServer(target.HumanoidRootPart)
        -- print("[Attack] Tentando atacar via RemoteEvent 'Events.Attack' com alvo: " .. target.Name)
        wait(0.2) return
    end

    AttackRemote = ReplicatedStorage:FindFirstChild("CombatEvents") and ReplicatedStorage.CombatEvents:FindFirstChild("Damage")
    if AttackRemote and AttackRemote:IsA("RemoteEvent") then
        AttackRemote:FireServer(target.HumanoidRootPart, "M1")
        -- print("[Attack] Tentando atacar via RemoteEvent 'CombatEvents.Damage' com alvo e tipo: " .. target.Name)
        wait(0.2) return
    end
    
    warn("[Attack] Nenhum RemoteEvent de ataque funcional encontrado! Tentando simular clique do mouse (provavelmente não dará dano).")
    game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Down:fire()
    wait(0.1)
    game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Up:fire()
    wait(0.2)
end

function InteractWithNPC(npcName, cframe)
    local npc = game:GetService("Workspace"):FindFirstChild(npcName)
    if npc and npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") then
        local LocalPlayer = game:GetService("Players").LocalPlayer
        local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

        if PlayerHRP then
            Teleport(cframe or npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 5)) -- Usa a CFrame fornecida ou calcula uma próxima
            wait(0.5)

            local ReplicatedStorage = game:GetService("ReplicatedStorage")
            local InteractRemote = nil

            -- TENTATIVAS DE ENCONTRAR O REMOTE EVENT DE INTERAÇÃO COM NPC
            InteractRemote = ReplicatedStorage:FindFirstChild("Remote")
            if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                InteractRemote:FireServer("Quest", npc.Name)
                -- print("[NPC Interaction] Tentando interagir com NPC '" .. npc.Name .. "' via 'Remote' com 'Quest'.")
                wait(0.5) return
            end

            InteractRemote = ReplicatedStorage:FindFirstChild("Remote")
            if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                InteractRemote:FireServer("Evt", npc.Name, "Click")
                -- print("[NPC Interaction] Tentando interagir com NPC '" .. npc.Name .. "' via 'Remote' com 'Evt', 'Click'.")
                wait(0.5) return
            end
            
            InteractRemote = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Interact")
            if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                InteractRemote:FireServer(npc.Name, "Quest")
                -- print("[NPC Interaction] Tentando interagir com NPC '" .. npc.Name .. "' via 'Events.Interact'.")
                wait(0.5) return
            end

            warn("[NPC Interaction] Nenhum RemoteEvent de interação funcional encontrado para '" .. npc.Name .. "'! A interação com o NPC pode falhar.")
        end
    else
        warn("[NPC Interaction] NPC '" .. npcName .. "' não encontrado para interação. Verifique o nome do NPC e a CFrame.")
    end
end

-- =====================================================================================================
--                            Bloco 3: Funções de Auto-Farm por Quest/Nível
-- =====================================================================================================

-- Função genérica de farm para ser usada por cada nível específico
local function runFarmCycle(questData)
    local mobName = questData.Mon
    local questNPCName = questData.NPCName
    local questCFrame = questData.CFrameQuest
    local mobSpawnCFrame = questData.CFrameMon
    local mobsToKill = questData.MobsToKill or 5 -- Padrão de 5 mobs por quest

    print("\n--- Ciclo de Farm para Quest: " .. (questData.NameQuest or "Desconhecida") .. " ---")

    -- 1. Aceitar/Completar Quest (se _G.AutoQuest estiver ativado)
    if _G.AutoQuest and questNPCName and questCFrame then
        print("[AutoQuest] Teletransportando para o NPC da Quest: " .. questNPCName)
        Teleport(questCFrame)
        wait(1.5)
        InteractWithNPC(questNPCName, questCFrame) -- Passa a CFrame para Interagir
        wait(1.5)
    elseif _G.AutoQuest then
        warn("[AutoQuest] Não foi possível interagir com o NPC da quest. Verifique NPCName e CFrameQuest.")
    end

    -- 2. Teletransportar para o local de spawn dos monstros
    if mobName and mobSpawnCFrame then
        print("[Farm] Teletransportando para a área de farm de: " .. mobName)
        Teleport(mobSpawnCFrame)
        wait(1.5)

        local mobsKilled = 0
        local attemptCount = 0

        while mobsKilled < mobsToKill and _G.CurrentFarmActive and attemptCount < (mobsToKill * 20) do
            _G.TargetMob = FindMob(mobName)

            if _G.TargetMob then
                print("[Farm] Atacando: " .. _G.TargetMob.Name .. " (" .. mobsKilled .. "/" .. mobsToKill .. " abatidos)")
                if _G.AutoAttack then
                    AttackTarget(_G.TargetMob)
                end
                
                local attackAttempts = 0
                local LocalPlayer = game:GetService("Players").LocalPlayer
                while _G.TargetMob and _G.TargetMob:FindFirstChild("Humanoid") and _G.TargetMob.Humanoid.Health > 0 and attackAttempts < 50 and _G.CurrentFarmActive do
                    if _G.AutoAttack then
                        AttackTarget(_G.TargetMob)
                    end
                    wait(0.2)
                    attackAttempts = attackAttempts + 1
                    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and _G.TargetMob:FindFirstChild("HumanoidRootPart") then
                        if (LocalPlayer.Character.HumanoidRootPart.Position - _G.TargetMob.HumanoidRootPart.Position).magnitude > 20 then
                            Teleport(_G.TargetMob.HumanoidRootPart.CFrame * CFrame.new(0,0,5))
                            wait(0.5)
                        end
                    end
                end

                if not _G.TargetMob or not _G.TargetMob:FindFirstChild("Humanoid") or _G.TargetMob.Humanoid.Health <= 0 then
                    print("[Farm] Monstro derrotado: " .. mobName)
                    mobsKilled = mobsKilled + 1
                    _G.TargetMob = nil
                    wait(0.5)
                else
                    warn("[Farm] Monstro '" .. mobName .. "' não derrotado ou problema no alvo. Buscando próximo. Saúde restante: " .. _G.TargetMob.Humanoid.Health)
                    _G.TargetMob = nil
                end
            else
                print("[Farm] Nenhum monstro '" .. mobName .. "' encontrado. Esperando o spawn ou procurando novamente...")
                wait(2)
            end
            attemptCount = attemptCount + 1
            wait(0.1)
        end

        if mobsKilled >= mobsToKill then
            print("[Farm] Mobs suficientes derrotados (" .. mobsKilled .. "/" .. mobsToKill .. ") para a quest de " .. mobName .. ".")
        else
            warn("[Farm] Não foi possível derrotar todos os mobs para a quest atual. Abatidos: " .. mobsKilled .. ". Verifique se as CFrames e os nomes dos mobs estão corretos ou se os remotes de ataque estão funcionando.")
        end
    else
        warn("[Farm] Não foi possível iniciar o farm de mobs. Verifique o nome do monstro e a CFrame de spawn.")
    end

    -- 3. Voltar para o NPC da Quest para entregar (se _G.AutoQuest estiver ativado)
    if _G.AutoQuest and questNPCName and questCFrame then
        print("[AutoQuest] Voltando para o NPC da Quest para entregar: " .. questNPCName)
        Teleport(questCFrame)
        wait(1.5)
        InteractWithNPC(questNPCName, questCFrame)
        wait(1.5)
    elseif _G.AutoQuest then
        warn("[AutoQuest] Não foi possível entregar a quest. Verifique NPCName e CFrameQuest.")
    end

    wait(2)
end

-- Função para iniciar qualquer farm
local function StartFarm(farmType, questData)
    if _G.CurrentFarmActive then
        warn("Já existe um auto farm ativo. Por favor, pare o farm atual antes de iniciar um novo.")
        return
    end

    _G.CurrentFarmActive = true
    _G.FarmType = farmType
    updateStatusText("Status: Ativo (" .. farmType .. ")")
    print("====================================")
    print("           " .. farmType .. " INICIADO!      ")
    print("====================================")

    autoFarmThread = spawn(function()
        while _G.CurrentFarmActive and _G.FarmType == farmType do -- Garante que o loop só rode para o farm selecionado
            local playerLevel = game:GetService("Players").LocalPlayer.Data.Level.Value
            if playerLevel >= questData.MinLevel and playerLevel <= questData.MaxLevel then
                runFarmCycle(questData)
            else
                print("[Farm] Nível do jogador (" .. playerLevel .. ") fora da faixa para " .. farmType .. " (" .. questData.MinLevel .. "-" .. questData.MaxLevel .. ").")
                print("[Farm] Parando o farm. Selecione uma quest apropriada para seu nível.")
                StopFarm()
            end
            wait(0.5) -- Pequeno delay para evitar sobrecarga
        end
        print("====================================")
        print("          " .. farmType .. " PARADO.         ")
        print("====================================")
        if _G.FarmType == farmType then -- Apenas se este for o farm que foi explicitamente parado
            updateStatusText("Status: Inativo")
            _G.FarmType = nil
            autoFarmThread = nil
        end
    end)
end

-- Função para parar qualquer farm ativo
function StopFarm()
    if not _G.CurrentFarmActive then
        print("Nenhum auto farm está ativo.")
        return
    end
    print("[Controle] Sinal para parar Auto Farm enviado.")
    _G.CurrentFarmActive = false -- Isso vai encerrar o loop 'while _G.CurrentFarmActive'
    _G.TargetMob = nil
    -- O restante da limpeza e atualização da GUI será feita na própria thread do farm quando ela terminar
end

-- =====================================================================================================
--                            Bloco 4: Definições de Quests por Nível
-- =====================================================================================================

-- Definição de dados para cada quest/nível.
-- Crie uma tabela para cada quest, com todas as informações necessárias.
local QuestDefinitions = {
    -- PRIMEIRO MAR
    Bandit = {
        MinLevel = 1,
        MaxLevel = 9,
        Mon = "Bandit",
        NameQuest = "Bandit Quest",
        NameMon = "Bandit",
        NPCName = "Bandit Quest Giver",
        MobsToKill = 5, -- Ou a quantidade que você quiser por ciclo
        CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544), -- CFrame do NPC
        CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125) -- CFrame da área de spawn
    },
    Gorilla = {
        MinLevel = 10,
        MaxLevel = 19,
        Mon = "Gorilla",
        NameQuest = "Gorilla Quest",
        NameMon = "Gorilla",
        NPCName = "Gorilla Quest Giver",
        MobsToKill = 5,
        CFrameQuest = CFrame.new(1471.74866, 15.4495068, 1618.173, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544),
        CFrameMon = CFrame.new(1495.60266, 27.00250816345215, 1632.74878)
    },
    -- ADICIONE MAIS QUESTS AQUI SEGUINDO O PADRÃO:
    -- Exemplo para o Segundo Mar:
    MarineCaptain = {
        MinLevel = 700,
        MaxLevel = 749,
        Mon = "Marine Captain",
        NameQuest = "Marine Captain Quest",
        NameMon = "Marine Captain",
        NPCName = "Marine Captain Quest Giver",
        MobsToKill = 5,
        CFrameQuest = CFrame.new(0,0,0), -- <<<<< ATUALIZE AQUI
        CFrameMon = CFrame.new(0,0,0)    -- <<<<< ATUALIZE AQUI
    },
    -- Exemplo para o Terceiro Mar:
    IceAdmiral = {
        MinLevel = 1500,
        MaxLevel = 1549,
        Mon = "Ice Admiral",
        NameQuest = "Ice Admiral Quest",
        NameMon = "Ice Admiral",
        NPCName = "Ice Admiral Quest Giver",
        MobsToKill = 5,
        CFrameQuest = CFrame.new(0,0,0), -- <<<<< ATUALIZE AQUI
        CFrameMon = CFrame.new(0,0,0)    -- <<<<< ATUALIZE AQUI
    },
    -- ETC.
}

-- =====================================================================================================
--                            Bloco 5: Threads e Listeners Secundários
-- =====================================================================================================

spawn(function()
    game:GetService('RunService').Stepped:Connect(function()
        if _G.Remove_Effect then
            for i, v in pairs(game:GetService("ReplicatedStorage").Effect.Container:GetChildren()) do
                if v.Name == "Death" then
                    v:Destroy()
                end
            end
        end
    end)
end)

spawn(function()
    while wait(5) do
        for i,v in pairs(game.Players:GetPlayers()) do
            if v.Name == "red_game43" or v.Name == "rip_indra" or v.Name == "Axiore" or v.Name == "Polkster" or v.Name == "wenlocktoad" or v.Name == "Daigrock" or v.Name == "toilamvidamme" or v.Name == "oofficialnoobie" or v.Name == "Uzoth" or v.Name == "Azarth" or v.Name == "arlthmetic" or v.Name == "Death_King" or v.Name == "Lunoven" or v.Name == "TheGreateAced" or v.Name == "rip_fud" or v.Name == "drip_mama" or v.Name == "layandikit12" or v.Name == "Hingoi" then
                warn("[Anti-Admin] Admin/Player específico detectado: " .. v.Name .. ". Dando Hop!")
                Hop()
                wait(20)
                break
            end
        end
    end
end)

spawn(function()
    while wait(15) do
        if _G.CurrentFarmActive then
            local LocalPlayer = game:GetService("Players").LocalPlayer
            local Humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
            if Humanoid then
                Humanoid:ChangeState(Enum.HumanoidStateType.Running)
                wait(0.1)
                Humanoid:ChangeState(Enum.HumanoidStateType.Idle)
            end
        end
    end
end)

-- =====================================================================================================
--                            Bloco 6: Interface Gráfica (GUI)
-- =====================================================================================================

local player = game:GetService("Players").LocalPlayer
local coreGui = game:GetService("CoreGui")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AutoFarmGUI_Detailed"
ScreenGui.Parent = coreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 250, 0, 350) -- Aumentado para acomodar mais botões
MainFrame.Position = UDim2.new(0.5, -125, 0.5, -175) -- Centraliza o frame
MainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MainFrame.BorderSizePixel = 0
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "TitleLabel"
TitleLabel.Size = UDim2.new(1, 0, 0, 30)
TitleLabel.Position = UDim2.new(0, 0, 0, 0)
TitleLabel.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
TitleLabel.Text = "⚡ Auto Farm Blox Fruits ⚡"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.Font = Enum.Font.SourceSansBold
TitleLabel.TextSize = 18
TitleLabel.BorderSizePixel = 0
TitleLabel.Parent = MainFrame

local StatusLabel = Instance.new("TextLabel")
StatusLabel.Name = "StatusLabel"
StatusLabel.Size = UDim2.new(1, 0, 0, 20)
StatusLabel.Position = UDim2.new(0, 0, 0, 30)
StatusLabel.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
StatusLabel.Text = "Status: Inativo"
StatusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
StatusLabel.Font = Enum.Font.SourceSans
StatusLabel.TextSize = 14
StatusLabel.BorderSizePixel = 0
StatusLabel.Parent = MainFrame

local function updateStatusText(text)
    StatusLabel.Text = text
end

local function updateToggleButton(button, value)
    if value then
        button.Text = button.Name .. ": ON"
        button.BackgroundColor3 = Color3.fromRGB(70, 150, 70) -- Verde para ON
    else
        button.Text = button.Name .. ": OFF"
        button.BackgroundColor3 = Color3.fromRGB(150, 70, 70) -- Vermelho para OFF
    end
end

-- Botão Toggle Auto Quest
local AutoQuestButton = Instance.new("TextButton")
AutoQuestButton.Name = "AutoQuest"
AutoQuestButton.Size = UDim2.new(0.9, 0, 0, 25)
AutoQuestButton.Position = UDim2.new(0.05, 0, 0, 60)
AutoQuestButton.Font = Enum.Font.SourceSansBold
AutoQuestButton.TextSize = 14
AutoQuestButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoQuestButton.BorderSizePixel = 0
AutoQuestButton.Parent = MainFrame
updateToggleButton(AutoQuestButton, _G.AutoQuest)

AutoQuestButton.MouseButton1Click:Connect(function()
    _G.AutoQuest = not _G.AutoQuest
    updateToggleButton(AutoQuestButton, _G.AutoQuest)
    print("[Controle] AutoQuest agora está: " .. tostring(_G.AutoQuest))
end)

-- Botão Toggle Auto Attack
local AutoAttackButton = Instance.new("TextButton")
AutoAttackButton.Name = "AutoAttack"
AutoAttackButton.Size = UDim2.new(0.9, 0, 0, 25)
AutoAttackButton.Position = UDim2.new(0.05, 0, 0, 90) -- Abaixo do AutoQuest
AutoAttackButton.Font = Enum.Font.SourceSansBold
AutoAttackButton.TextSize = 14
AutoAttackButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoAttackButton.BorderSizePixel = 0
AutoAttackButton.Parent = MainFrame
updateToggleButton(AutoAttackButton, _G.AutoAttack)

AutoAttackButton.MouseButton1Click:Connect(function()
    _G.AutoAttack = not _G.AutoAttack
    updateToggleButton(AutoAttackButton, _G.AutoAttack)
    print("[Controle] AutoAttack agora está: " .. tostring(_G.AutoAttack))
end)

-- Separador visual
local Separator = Instance.new("Frame")
Separator.Name = "Separator"
Separator.Size = UDim2.new(1, 0, 0, 2)
Separator.Position = UDim2.new(0, 0, 0, 120)
Separator.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
Separator.Parent = MainFrame

-- ScrollFrame para os botões de níveis (se houver muitos)
local LevelButtonsFrame = Instance.new("ScrollingFrame")
LevelButtonsFrame.Name = "LevelButtonsFrame"
LevelButtonsFrame.Size = UDim2.new(1, 0, 0.5, -120) -- Ocupa o resto do espaço
LevelButtonsFrame.Position = UDim2.new(0, 0, 0, 125)
LevelButtonsFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
LevelButtonsFrame.BorderSizePixel = 0
LevelButtonsFrame.CanvasSize = UDim2.new(0, 0, 0, 0) -- Será ajustado dinamicamente
LevelButtonsFrame.VerticalScrollBarInset = Enum.ScrollBarInset.Always
LevelButtonsFrame.ScrollingDirection = Enum.ScrollingDirection.Y
LevelButtonsFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Name = "QuestButtonLayout"
UIListLayout.Padding = UDim.new(0, 5) -- Espaçamento entre os botões
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIListLayout.VerticalAlignment = Enum.VerticalAlignment.Top
UIListLayout.Parent = LevelButtonsFrame

-- Função para criar um botão de quest dinamicamente
local function createQuestButton(questName, questData, yPosition)
    local button = Instance.new("TextButton")
    button.Name = questName .. "FarmButton"
    button.Size = UDim2.new(0.9, 0, 0, 30)
    button.Position = UDim2.new(0.05, 0, 0, yPosition) -- Posição inicial, será ajustada pelo layout
    button.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    button.Text = "Farm " .. questData.NameQuest .. " (Lvl " .. questData.MinLevel .. "-" .. questData.MaxLevel .. ")"
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 14
    button.BorderSizePixel = 0
    button.Parent = LevelButtonsFrame

    button.MouseButton1Click:Connect(function()
        if _G.CurrentFarmActive then
            if _G.FarmType == questName then
                StopFarm() -- Se já está ativo para esta quest, para
                print("Parando " .. questName .. " farm.")
            else
                warn("Já existe um farm ativo (" .. _G.FarmType .. "). Por favor, pare-o primeiro.")
                -- Opcional: Parar o farm atual e iniciar o novo
                -- StopFarm()
                -- spawn(function() wait(1) StartFarm(questName, questData) end)
            end
        else
            StartFarm(questName, questData)
        end
    end)
    return button
end

-- Adiciona os botões de quest dinamicamente
local currentY = 0
local buttonHeight = 30
local padding = 5

for questName, questData in pairs(QuestDefinitions) do
    if (World1 and questData.MinLevel < 700) or (World2 and questData.MinLevel >= 700 and questData.MinLevel < 1500) or (World3 and questData.MinLevel >= 1500) then
        -- Cria o botão e adiciona ao LevelButtonsFrame.
        -- O UIListLayout se encarregará da posição, então yPosition é menos crítico aqui,
        -- mas é bom ter uma ideia se você for remover o layout.
        createQuestButton(questName, questData, currentY)
        currentY = currentY + buttonHeight + padding
    end
end

-- Ajusta o CanvasSize do ScrollingFrame para que todos os botões caibam
LevelButtonsFrame.CanvasSize = UDim2.new(0, 0, 0, currentY)


-- Botão STOP ALL FARM
local StopAllFarmButton = Instance.new("TextButton")
StopAllFarmButton.Name = "Stop All Farm"
StopAllFarmButton.Size = UDim2.new(0.9, 0, 0, 30)
StopAllFarmButton.Position = UDim2.new(0.05, 0, 0, MainFrame.Size.Y.Offset - 35) -- Posiciona no final do frame
StopAllFarmButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50) -- Vermelho vibrante
StopAllFarmButton.Text = "STOP ALL FARM"
StopAllFarmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
StopAllFarmButton.Font = Enum.Font.SourceSansBold
StopAllFarmButton.TextSize = 16
StopAllFarmButton.BorderSizePixel = 0
StopAllFarmButton.Parent = MainFrame

StopAllFarmButton.MouseButton1Click:Connect(function()
    StopFarm()
    updateStatusText("Status: Inativo")
end)

-- Botão para Esconder/Mostrar a GUI
local HideShowButton = Instance.new("TextButton")
HideShowButton.Name = "Hide/Show"
HideShowButton.Size = UDim2.new(0.4, 0, 0, 15)
HideShowButton.Position = UDim2.new(0.05, 0, 0, MainFrame.Size.Y.Offset - 15) -- Abaixo do StopAllFarmButton
HideShowButton.Font = Enum.Font.SourceSans
HideShowButton.TextSize = 12
HideShowButton.TextColor3 = Color3.fromRGB(255, 255, 255)
HideShowButton.BackgroundColor3 = Color3.fromRGB(90, 90, 90)
HideShowButton.Text = "Hide"
HideShowButton.BorderSizePixel = 0
HideShowButton.Parent = MainFrame

HideShowButton.MouseButton1Click:Connect(function()
    if MainFrame.Visible then
        MainFrame.Visible = false
        HideShowButton.Text = "Show"
    else
        MainFrame.Visible = true
        HideShowButton.Text = "Hide"
    end
end)

-- Botão para Sair (Destruir a GUI e parar o script)
local ExitButton = Instance.new("TextButton")
ExitButton.Name = "Exit"
ExitButton.Size = UDim2.new(0.4, 0, 0, 15)
ExitButton.Position = UDim2.new(0.55, 0, 0, MainFrame.Size.Y.Offset - 15)
ExitButton.Font = Enum.Font.SourceSans
ExitButton.TextSize = 12
ExitButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ExitButton.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
ExitButton.Text = "Exit"
ExitButton.BorderSizePixel = 0
ExitButton.Parent = MainFrame

ExitButton.MouseButton1Click:Connect(function()
    StopFarm()
    ScreenGui:Destroy()
    warn("Script de Auto Farm finalizado e GUI destruída.")
end)

-- Inicializa o status na GUI
updateStatusText("Status: Inativo")

-- Mensagem de inicialização
print("Auto Farm GUI carregada. Selecione uma opção de farm.")
