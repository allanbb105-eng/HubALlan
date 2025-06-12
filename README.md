-- Verifica o PlaceId para determinar o mundo
local World1 = false
local World2 = false
local World3 = false

if game.PlaceId == 2753915549 then -- Primeiro Mar (First Sea)
    World1 = true
elseif game.PlaceId == 4442272183 then -- Segundo Mar (Second Sea)
    World2 = true
elseif game.PlaceId == 7449423635 then -- Terceiro Mar (Third Sea)
    World3 = true
else
    game:GetService("Players").LocalPlayer:Kick("Local não suportado. Por favor, entre em um dos mares suportados (Blox Fruits).")
end

-- Variáveis globais (_G) para controle do script
_G.Remove_Effect = true  -- Para remover o efeito de morte
_G.AutoFarm = false      -- Controla se o auto-farm está ativo
_G.AutoQuest = true      -- Controla se o script deve aceitar e entregar quests automaticamente
_G.AutoAttack = true     -- Controla se o script deve atacar monstros automaticamente
_G.TargetMob = nil       -- Referência ao monstro alvo atual

-- --- FUNÇÕES AUXILIARES GERAIS ---

-- Função Hop() para trocar de servidor (para evitar admins/players)
-- Use a função nativa do seu executor se houver uma melhor (ex: teleport.toRandomServer())
function Hop()
    print("[Hop] Tentando trocar de servidor...")
    local TeleportService = game:GetService("TeleportService")
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer

    if LocalPlayer then
        TeleportService:Teleport(game.PlaceId, LocalPlayer)
    else
        warn("[Hop] LocalPlayer não encontrado, não é possível teletransportar.")
    end
end

-- Função para teletransportar o jogador
function Teleport(cframe)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    if LocalPlayer and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = cframe
        print("[Teleport] Teletransportado para: X=" .. math.floor(cframe.Position.X) .. ", Y=" .. math.floor(cframe.Position.Y) .. ", Z=" .. math.floor(cframe.Position.Z))
    else
        warn("[Teleport] HumanoidRootPart do jogador não encontrada para teletransporte.")
    end
end

-- Função para encontrar o monstro mais próximo pelo nome
function FindMob(name)
    local workspace = game:GetService("Workspace")
    local foundMob = nil
    local shortestDistance = math.huge
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    if not PlayerHRP then return nil end

    for i, v in pairs(workspace:GetChildren()) do
        -- Verifica se é um modelo, tem o nome correto, tem Humanoid e está vivo
        if v:IsA("Model") and v.Name:find(name, 1, true) and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 and v:FindFirstChild("HumanoidRootPart") then
            local mobHRP = v.HumanoidRootPart
            local distance = (PlayerHRP.Position - mobHRP.Position).magnitude

            if distance < shortestDistance then
                shortestDistance = distance
                foundMob = v
            end
        end
    end
    return foundMob -- Retorna o monstro mais próximo
end

-- Função para atacar um alvo (usando RemoteEvents para interação com o servidor)
function AttackTarget(target)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    if not (PlayerHRP and target and target:FindFirstChild("HumanoidRootPart") and target:FindFirstChild("Humanoid") and target.Humanoid.Health > 0) then
        return
    end

    -- Aponta o personagem para o alvo
    PlayerHRP.CFrame = CFrame.new(PlayerHRP.Position, target.HumanoidRootPart.Position)

    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    -- *** TENTATIVAS DE REMOTE EVENTS PARA ATAQUE (M1) ***
    -- Tente estas variações. O Remote Spy dirá qual é a correta.
    local AttackRemote = ReplicatedStorage:FindFirstChild("Remote") -- Nome comum
    if not AttackRemote then AttackRemote = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Attack") end
    if not AttackRemote then AttackRemote = ReplicatedStorage:FindFirstChild("CombatEvents") and ReplicatedStorage.CombatEvents:FindFirstChild("Damage") end
    -- Adicione outras variações se o Remote Spy mostrar nomes diferentes!

    if AttackRemote and AttackRemote:IsA("RemoteEvent") then
        -- *** ARGUMENTOS PARA ATAQUE (M1) ***
        -- Estes são exemplos comuns para Blox Fruits. O Remote Spy é a fonte final da verdade.
        AttackRemote:FireServer("Attack", target.HumanoidRootPart) -- Variação 1 (muito comum)
        -- AttackRemote:FireServer("DoDamage", target.HumanoidRootPart, "M1") -- Variação 2
        -- AttackRemote:FireServer(target.HumanoidRootPart) -- Variação 3 (apenas o alvo)
        -- AttackRemote:FireServer("Hit", target) -- Variação 4

        print("[Attack] Tentando atacar via RemoteEvent: " .. target.Name)
    else
        warn("[Attack] Remote de ataque não encontrado ou inválido! Atacando via clique do mouse (pode não dar dano).")
        -- Fallback: Simula um clique do mouse (provavelmente não dará dano real em Blox Fruits)
        game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Down:fire()
        wait(0.1)
        game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Up:fire()
    end
    wait(0.2) -- Pequeno delay entre ataques para evitar flood
end

-- --- LÓGICA DE QUESTS (CheckQuest agora retorna uma tabela com dados da quest) ---
-- *** VOCÊ PRECISA ATUALIZAR AS CFrames AQUI! ***
function CheckQuest()
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local MyLevel = LocalPlayer.Data.Level.Value
    local questData = {}

    if World1 then
        if MyLevel >= 1 and MyLevel <= 9 then
            questData = {
                Mon = "Bandit", -- Nome do modelo do monstro no Workspace
                LevelQuest = 1,
                NameQuest = "Bandit Quest", -- Nome da quest para interface
                NameMon = "Bandit", -- Nome para encontrar o monstro
                NPCName = "Bandit Quest Giver", -- Nome do NPC
                CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544), -- CFrame do NPC
                CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125) -- CFrame da área de spawn do monstro
            }
        elseif MyLevel >= 10 and MyLevel <= 19 then
            questData = {
                Mon = "Gorilla",
                LevelQuest = 10,
                NameQuest = "Gorilla Quest",
                NameMon = "Gorilla",
                NPCName = "Gorilla Quest Giver",
                CFrameQuest = CFrame.new(1471.74866, 15.4495068, 1618.173, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544),
                CFrameMon = CFrame.new(1495.60266, 27.00250816345215, 1632.74878)
            }
        -- Exemplo para o próximo conjunto de níveis em World1:
        elseif MyLevel >= 20 and MyLevel <= 29 then
            questData = {
                Mon = "Jungle Pirate",
                LevelQuest = 20,
                NameQuest = "Jungle Pirate Quest",
                NameMon = "Jungle Pirate",
                NPCName = "Jungle Pirate Quest Giver",
                CFrameQuest = CFrame.new(0,0,0), -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
                CFrameMon = CFrame.new(0,0,0) -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
            }
        -- [[ ADICIONE MAIS QUESTS PARA WORLD1 AQUI (copie e cole o bloco elseif) ]]
        end
    elseif World2 then
        -- Exemplo para World2:
        if MyLevel >= 700 and MyLevel <= 749 then
            questData = {
                Mon = "Marine Captain",
                LevelQuest = 700,
                NameQuest = "Marine Captain Quest",
                NameMon = "Marine Captain",
                NPCName = "Marine Captain Quest Giver",
                CFrameQuest = CFrame.new(0,0,0), -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
                CFrameMon = CFrame.new(0,0,0) -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
            }
        -- [[ ADICIONE MAIS QUESTS PARA WORLD2 AQUI ]]
        end
    elseif World3 then
        -- Exemplo para World3:
        if MyLevel >= 1500 and MyLevel <= 1549 then
            questData = {
                Mon = "Ice Admiral",
                LevelQuest = 1500,
                NameQuest = "Ice Admiral Quest",
                NameMon = "Ice Admiral",
                NPCName = "Ice Admiral Quest Giver",
                CFrameQuest = CFrame.new(0,0,0), -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
                CFrameMon = CFrame.new(0,0,0) -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
            }
        -- [[ ADICIONE MAIS QUESTS PARA WORLD3 AQUI ]]
        end
    end
    return questData
end

-- Função para interagir com o NPC (Aceitar/Completar Quest)
function InteractWithNPC(npcName)
    local npc = game:GetService("Workspace"):FindFirstChild(npcName)
    if npc and npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") then
        local LocalPlayer = game:GetService("Players").LocalPlayer
        local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

        if PlayerHRP then
            Teleport(npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 5))
            wait(0.5)

            local ReplicatedStorage = game:GetService("ReplicatedStorage")
            -- *** TENTATIVAS DE REMOTE EVENTS PARA INTERAÇÃO COM NPC ***
            -- Tente estas variações. O Remote Spy dirá qual é a correta.
            local InteractRemote = ReplicatedStorage:FindFirstChild("Remote") -- Nome comum
            if not InteractRemote then InteractRemote = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Interact") end
            if not InteractRemote then InteractRemote = ReplicatedStorage:FindFirstChild("QuestHandler") and ReplicatedStorage.QuestHandler:FindFirstChild("AcceptQuest") end
            -- Adicione outras variações se o Remote Spy mostrar nomes diferentes!

            if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                -- *** ARGUMENTOS PARA INTERAÇÃO COM NPC ***
                -- Estes são exemplos comuns para Blox Fruits. O Remote Spy é a fonte final da verdade.
                InteractRemote:FireServer("Quest", npc.Name) -- Variação 1 (muito comum para quests)
                -- InteractRemote:FireServer("Evt", npc.Name, "Click") -- Variação 2 (simula clique no NPC)
                -- InteractRemote:FireServer("Talk", npc.Name) -- Variação 3
                -- InteractRemote:FireServer("Accept", questData.NameQuest, npc.Name) -- Variação 4 (se o nome da quest for argumento)
                -- InteractRemote:FireServer("Complete", npc.Name) -- Variação 5 (para entregar)
                
                print("[NPC Interaction] Tentando interagir com NPC: " .. npc.Name)
            else
                warn("[NPC Interaction] Remote de interação não encontrado ou inválido! A interação com o NPC pode falhar.")
            end
            wait(0.5)
        end
    else
        warn("[NPC Interaction] NPC '" .. npcName .. "' não encontrado para interação.")
    end
end

-- --- FUNÇÃO PRINCIPAL DE AUTO-FARM ---

local autoFarmThread = nil -- Variável para armazenar a thread do auto-farm

function StartAutoFarm()
    if _G.AutoFarm and autoFarmThread then
        print("[AutoFarm] Auto Farm já está em execução.")
        return
    end

    _G.AutoFarm = true
    print("====================================")
    print("           Auto Farm INICIADO!      ")
    print("====================================")
    updateStatusText("Status: Ativo") -- Atualiza a GUI

    autoFarmThread = spawn(function()
        while _G.AutoFarm do
            local questData = CheckQuest()

            if not questData.Mon or not questData.CFrameQuest or not questData.CFrameMon then
                warn("Nenhuma quest encontrada para o seu nível atual ou dados de CFrame incompletos. Verifique a função CheckQuest(). Parando Auto Farm.")
                StopAutoFarm()
                break
            end

            local questNPCName = questData.NPCName
            local mobName = questData.Mon
            local questCFrame = questData.CFrameQuest
            local mobSpawnCFrame = questData.CFrameMon
            local mobsToKill = 5 -- Quantidade de mobs para matar por quest (AJUSTE CONFORME O JOGO E A QUEST)

            print("\n--- Ciclo de Farm para Nível " .. game:GetService("Players").LocalPlayer.Data.Level.Value .. " (Quest: " .. questData.NameQuest .. ") ---")

            -- 1. Aceitar/Completar Quest (se _G.AutoQuest estiver ativado)
            if _G.AutoQuest and questNPCName and questCFrame then
                print("[AutoQuest] Teletransportando para o NPC da Quest: " .. questNPCName)
                Teleport(questCFrame)
                wait(1.5) -- Espera para carregamento
                InteractWithNPC(questNPCName)
                wait(1.5) -- Espera após interagir com o NPC
            end

            -- 2. Teletransportar para o local de spawn dos monstros
            print("[Farm] Teletransportando para a área de farm de: " .. mobName)
            Teleport(mobSpawnCFrame)
            wait(1.5) -- Espera para o carregamento do mapa/mobs

            -- 3. Encontrar e atacar monstros
            local mobsKilled = 0
            local attemptCount = 0 -- Contador de tentativas para evitar loops infinitos caso o mob não apareça

            while mobsKilled < mobsToKill and _G.AutoFarm and attemptCount < (mobsToKill * 20) do -- Limita tentativas de encontrar mob
                _G.TargetMob = FindMob(mobName)

                if _G.TargetMob then
                    print("[Farm] Atacando: " .. _G.TargetMob.Name .. " (" .. mobsKilled .. "/" .. mobsToKill .. " abatidos)")
                    if _G.AutoAttack then
                        AttackTarget(_G.TargetMob)
                    end
                    
                    local attackAttempts = 0
                    while _G.TargetMob and _G.TargetMob:FindFirstChild("Humanoid") and _G.TargetMob.Humanoid.Health > 0 and attackAttempts < 50 and _G.AutoFarm do -- Limita tentativas de ataque
                        if _G.AutoAttack then
                            AttackTarget(_G.TargetMob)
                        end
                        wait(0.2) -- Pequeno delay entre ataques
                        attackAttempts = attackAttempts + 1
                        -- Se o alvo estiver muito longe após um teleporte, tenta se reaproximar
                        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and _G.TargetMob:FindFirstChild("HumanoidRootPart") and (LocalPlayer.Character.HumanoidRootPart.Position - _G.TargetMob.HumanoidRootPart.Position).magnitude > 20 then
                            Teleport(_G.TargetMob.HumanoidRootPart.CFrame * CFrame.new(0,0,5)) -- Teleporta para perto do alvo
                            wait(0.5) -- Espera um pouco após o re-teleporte
                        end
                    end

                    if not _G.TargetMob or not _G.TargetMob:FindFirstChild("Humanoid") or _G.TargetMob.Humanoid.Health <= 0 then
                        print("[Farm] Monstro derrotado: " .. mobName)
                        mobsKilled = mobsKilled + 1
                        _G.TargetMob = nil -- Limpa o alvo para buscar o próximo
                        wait(0.5) -- Pequeno delay antes de procurar o próximo
                    else
                        warn("[Farm] Monstro '" .. mobName .. "' não derrotado ou problema no alvo. Buscando próximo. Saúde restante: " .. _G.TargetMob.Humanoid.Health)
                        _G.TargetMob = nil -- Limpa o alvo para buscar outro
                    end
                else
                    print("[Farm] Nenhum monstro '" .. mobName .. "' encontrado. Esperando o spawn ou procurando novamente...")
                    wait(2) -- Espera um pouco antes de tentar encontrar novamente
                end
                attemptCount = attemptCount + 1
                wait(0.1)
            end

            if mobsKilled >= mobsToKill then
                print("[Farm] Mobs suficientes derrotados (" .. mobsKilled .. "/" .. mobsToKill .. ") para a quest de " .. mobName + ".")
            else
                warn("[Farm] Não foi possível derrotar todos os mobs para a quest atual. Abatidos: " .. mobsKilled .. ". Verifique se as CFrames e os nomes dos mobs estão corretos ou se os remotes de ataque estão funcionando.")
            end

            -- 4. Voltar para o NPC da Quest para entregar (se _G.AutoQuest estiver ativado)
            if _G.AutoQuest and questNPCName and questCFrame then
                print("[AutoQuest] Voltando para o NPC da Quest para entregar: " .. questNPCName)
                Teleport(questCFrame)
                wait(1.5)
                InteractWithNPC(questNPCName)
                wait(1.5)
            end

            wait(2) -- Pequeno delay antes de checar a próxima quest ou loop
        end
        print("====================================")
        print("          Auto Farm PARADO.         ")
        print("====================================")
        updateStatusText("Status: Inativo") -- Atualiza a GUI
        autoFarmThread = nil -- Reseta a thread
    end)
end

function StopAutoFarm()
    _G.AutoFarm = false
    _G.TargetMob = nil
    print("[Controle] Sinal para parar Auto Farm enviado.")
    updateStatusText("Status: Inativo") -- Atualiza a GUI
end

-- --- THREADS E LISTENERS SECUNDÁRIOS ---

-- Thread para remover efeitos de morte (para melhor visibilidade)
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

-- Thread para detectar administradores/jogadores específicos e dar Hop
spawn(function()
    while wait(5) do -- Verifica a cada 5 segundos
        for i,v in pairs(game.Players:GetPlayers()) do
            if v.Name == "red_game43" or v.Name == "rip_indra" or v.Name == "Axiore" or v.Name == "Polkster" or v.Name == "wenlocktoad" or v.Name == "Daigrock" or v.Name == "toilamvidamme" or v.Name == "oofficialnoobie" or v.Name == "Uzoth" or v.Name == "Azarth" or v.Name == "arlthmetic" or v.Name == "Death_King" or v.Name == "Lunoven" or v.Name == "TheGreateAced" or v.Name == "rip_fud" or v.Name == "drip_mama" or v.Name == "layandikit12" or v.Name == "Hingoi" then
                warn("[Anti-Admin] Admin/Player específico detectado: " .. v.Name .. ". Dando Hop!")
                Hop() -- Chama a função Hop
                wait(20) -- Espera um pouco após o hop para não loopar excessivamente
                break -- Sai do loop de players após detectar um admin e dar hop
            end
        end
    end
end)

-- Anti-AFK (para evitar ser chutado por inatividade)
spawn(function()
    while wait(15) do -- A cada 15 segundos
        if _G.AutoFarm then
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

-- --- INTERFACE GRÁFICA (GUI) ---

local player = game:GetService("Players").LocalPlayer
local coreGui = game:GetService("CoreGui") -- Geralmente o local para criar GUIs em exploits

-- Cria a ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AutoFarmGUI"
ScreenGui.Parent = coreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Cria o Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 220, 0, 180)
MainFrame.Position = UDim2.new(0.5, -110, 0.5, -90)
MainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MainFrame.BorderSizePixel = 0
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

-- Título
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

-- Status Label
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

-- Função para atualizar o texto e a cor dos botões toggle
local function updateToggleButton(button, value)
    if value then
        button.Text = button.Name .. ": ON"
        button.BackgroundColor3 = Color3.fromRGB(70, 150, 70) -- Verde
    else
        button.Text = button.Name .. ": OFF"
        button.BackgroundColor3 = Color3.fromRGB(150, 70, 70) -- Vermelho
    end
end

-- Botão Toggle Auto Farm
local AutoFarmButton = Instance.new("TextButton")
AutoFarmButton.Name = "AutoFarm"
AutoFarmButton.Size = UDim2.new(0.9, 0, 0, 30)
AutoFarmButton.Position = UDim2.new(0.05, 0, 0, 60)
AutoFarmButton.Font = Enum.Font.SourceSansBold
AutoFarmButton.TextSize = 16
AutoFarmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoFarmButton.BorderSizePixel = 0
AutoFarmButton.Parent = MainFrame
updateToggleButton(AutoFarmButton, _G.AutoFarm)

AutoFarmButton.MouseButton1Click:Connect(function()
    _G.AutoFarm = not _G.AutoFarm
    updateToggleButton(AutoFarmButton, _G.AutoFarm)
    if _G.AutoFarm then
        StartAutoFarm()
    else
        StopAutoFarm()
    end
end)

-- Botão Toggle Auto Quest
local AutoQuestButton = Instance.new("TextButton")
AutoQuestButton.Name = "AutoQuest"
AutoQuestButton.Size = UDim2.new(0.9, 0, 0, 30)
AutoQuestButton.Position = UDim2.new(0.05, 0, 0, 95)
AutoQuestButton.Font = Enum.Font.SourceSansBold
AutoQuestButton.TextSize = 16
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
AutoAttackButton.Size = UDim2.new(0.9, 0, 0, 30)
AutoAttackButton.Position = UDim2.new(0.05, 0, 0, 130)
AutoAttackButton.Font = Enum.Font.SourceSansBold
AutoAttackButton.TextSize = 16
AutoAttackButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoAttackButton.BorderSizePixel = 0
AutoAttackButton.Parent = MainFrame
updateToggleButton(AutoAttackButton, _G.AutoAttack)

AutoAttackButton.MouseButton1Click:Connect(function()
    _G.AutoAttack = not _G.AutoAttack
    updateToggleButton(AutoAttackButton, _G.AutoAttack)
    print("[Controle] AutoAttack agora está: " .. tostring(_G.AutoAttack))
end)

-- Botão para Hide/Show a GUI
local HideShowButton = Instance.new("TextButton")
HideShowButton.Name = "Hide/Show"
HideShowButton.Size = UDim2.new(0.4, 0, 0, 15)
HideShowButton.Position = UDim2.new(0.05, 0, 0, 165)
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

-- Botão para Sair (Destruir a GUI e parar script)
local ExitButton = Instance.new("TextButton")
ExitButton.Name = "Exit"
ExitButton.Size = UDim2.new(0.4, 0, 0, 15)
ExitButton.Position = UDim2.new(0.55, 0, 0, 165)
ExitButton.Font = Enum.Font.SourceSans
ExitButton.TextSize = 12
ExitButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ExitButton.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
ExitButton.Text = "Exit"
ExitButton.BorderSizePixel = 0
ExitButton.Parent = MainFrame

ExitButton.MouseButton1Click:Connect(function()
    StopAutoFarm() -- Garante que o auto-farm pare
    ScreenGui:Destroy() -- Remove a GUI
    warn("Script de Auto Farm finalizado e GUI destruída.")
end)

-- Inicializa o status na GUI
updateStatusText("Status: Inativo")
