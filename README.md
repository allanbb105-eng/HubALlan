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
    print("Detectado: Primeiro Mar (PlaceId: 2753915549)")
elseif game.PlaceId == 4442272183 then -- PlaceId do Segundo Mar
    World2 = true
    print("Detectado: Segundo Mar (PlaceId: 4442272183)")
elseif game.PlaceId == 7449423635 then -- PlaceId do Terceiro Mar
    World3 = true
    print("Detectado: Terceiro Mar (PlaceId: 7449423635)")
else
    -- ATENÇÃO: COMENTEI ESTA LINHA PARA EVITAR KICK DURANTE A DEPURACAO.
    -- game:GetService("Players").LocalPlayer:Kick("Local não suportado. Por favor, entre em um dos mares suportados (Blox Fruits).")
    print("AVISO: PlaceId não reconhecido. Certifique-se de estar em um dos mares do Blox Fruits. Seu PlaceId: " .. game.PlaceId)
end

-- Variáveis Globais (_G): Estas variáveis são acessíveis de qualquer parte do script.
-- Elas funcionam como "flags" para controlar o comportamento do auto-farm.
_G.Remove_Effect = true  -- Se for 'true', o script tentará remover o efeito de "morte" (tela escura/cinza).

_G.CurrentFarmActive = false -- Flag para saber se alguma função de farm está ativa.
_G.FarmType = nil            -- Armazena o tipo de farm ativo (ex: "BanditFarm", "GorillaFarm").

_G.AutoQuest = true          -- Controla se o script deve automaticamente pegar e entregar quests.
_G.AutoAttack = true         -- Controla se o script deve atacar os monstros (usado no auto-farm de quests).
_G.TargetMob = nil           -- Uma variável para armazenar uma referência ao monstro que está sendo alvo no momento.

_G.AutoClick = false         -- Nova flag para o Auto Click.
_G.AutoClickDelay = 0.05     -- Delay entre cliques para o Auto Click (em segundos).

local autoFarmThread = nil   -- Variável para manter a referência à thread de farm.
local autoClickThread = nil  -- Variável para manter a referência à thread de auto click.


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
    print("[Teleport] Chamado para CFrame: " .. tostring(cframe)) -- Mensagem de depuração
    local LocalPlayer = game:GetService("Players").LocalPlayer
    if LocalPlayer and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = cframe
        print("[Teleport] Teletransportado para: X=" .. math.floor(cframe.Position.X) .. ", Y=" .. math.floor(cframe.Position.Y) .. ", Z=" .. math.floor(cframe.Position.Z))
    else
        warn("[Teleport] HumanoidRootPart do jogador não encontrada para teletransporte. Personagem pode não estar carregado ou você morreu.")
        -- Adicione uma tentativa de respawn se o personagem não estiver carregado
        if LocalPlayer and not LocalPlayer.Character then
            print("[Teleport] Tentando forçar o respawn...")
            LocalPlayer:LoadCharacter()
            wait(2) -- Tempo para o personagem carregar
        end
    end
end

function FindMob(name)
    local workspace = game:GetService("Workspace")
    local foundMob = nil
    local shortestDistance = math.huge
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    if not PlayerHRP then
        print("[FindMob] Player HumanoidRootPart não encontrado. Não é possível procurar mob.") -- Depuração
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
    if foundMob then
        print("[FindMob] Mob '" .. name .. "' encontrado: " .. foundMob.Name) -- Depuração
    else
        print("[FindMob] Nenhum mob '" .. name .. "' encontrado.") -- Depuração
    end
    return foundMob
end

function AttackTarget(target)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    if not (PlayerHRP and target and target:FindFirstChild("HumanoidRootPart") and target:FindFirstChild("Humanoid") and target.Humanoid.Health > 0) then
        print("[AttackTarget] Condições de ataque não atendidas (PlayerHRP ou alvo inválido/morto).") -- Depuração
        return
    end

    PlayerHRP.CFrame = CFrame.new(PlayerHRP.Position, target.HumanoidRootPart.Position)

    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local AttackRemote = nil
    local attackSucceeded = false

    -- TENTATIVAS DE ENCONTRAR O REMOTE EVENT DE ATAQUE (MAIS COMUNS NO BLOX FRUITS)
    -- *** ISTO É CRÍTICO! USE UM REMOTE SPY SE NÃO ESTIVER FUNCIONANDO! ***
    AttackRemote = ReplicatedStorage:FindFirstChild("Remote")
    if AttackRemote and AttackRemote:IsA("RemoteEvent") then
        AttackRemote:FireServer("Attack", target.HumanoidRootPart) -- Argumentos comuns para ataque M1
        print("[Attack] Tentativa via 'Remote' com 'Attack'.") -- Depuração
        attackSucceeded = true
        wait(0.2)
    end

    if not attackSucceeded then
        AttackRemote = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Attack")
        if AttackRemote and AttackRemote:IsA("RemoteEvent") then
            AttackRemote:FireServer(target.HumanoidRootPart) -- Outros argumentos comuns
            print("[Attack] Tentativa via 'Events.Attack'.") -- Depuração
            attackSucceeded = true
            wait(0.2)
        end
    end

    if not attackSucceeded then
        AttackRemote = ReplicatedStorage:FindFirstChild("CombatEvents") and ReplicatedStorage.CombatEvents:FindFirstChild("Damage")
        if AttackRemote and AttackRemote:IsA("RemoteEvent") then
            AttackRemote:FireServer(target.HumanoidRootPart, "M1") -- Outros argumentos comuns
            print("[Attack] Tentativa via 'CombatEvents.Damage'.") -- Depuração
            attackSucceeded = true
            wait(0.2)
        end
    end

    if not attackSucceeded then
        warn("[Attack] Nenhum RemoteEvent de ataque funcional encontrado! Tentando simular clique do mouse (provavelmente não dará dano real).")
        game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Down:fire()
        wait(0.1)
        game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Up:fire()
        wait(0.2)
    end
end

function InteractWithNPC(npcName, cframe)
    print("[InteractWithNPC] Chamado para NPC: " .. npcName) -- Depuração
    local npc = game:GetService("Workspace"):FindFirstChild(npcName)
    if npc and npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") then
        local LocalPlayer = game:GetService("Players").LocalPlayer
        local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

        if PlayerHRP then
            Teleport(cframe or npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 5)) -- Usar CFrame fornecida ou calcular uma próxima
            wait(1.5)

            local ReplicatedStorage = game:GetService("ReplicatedStorage")
            local InteractRemote = nil
            local interactionSucceeded = false

            -- TENTATIVAS DE ENCONTRAR O REMOTE EVENT DE INTERAÇÃO COM NPC (MAIS COMUNS NO BLOX FRUITS)
            -- *** ISTO É CRÍTICO! USE UM REMOTE SPY SE NÃO ESTIVER FUNCIONANDO! ***
            InteractRemote = ReplicatedStorage:FindFirstChild("Remote")
            if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                InteractRemote:FireServer("Quest", npc.Name)
                print("[NPC Interaction] Tentativa via 'Remote' com 'Quest'.") -- Depuração
                interactionSucceeded = true
                wait(0.5)
            end

            if not interactionSucceeded then
                InteractRemote = ReplicatedStorage:FindFirstChild("Remote")
                if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                    InteractRemote:FireServer("Evt", npc.Name, "Click")
                    print("[NPC Interaction] Tentativa via 'Remote' com 'Evt', 'Click'.") -- Depuração
                    interactionSucceeded = true
                    wait(0.5)
                end
            end

            if not interactionSucceeded then
                InteractRemote = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Interact")
                if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                    InteractRemote:FireServer(npc.Name, "Quest")
                    print("[NPC Interaction] Tentativa via 'Events.Interact'.") -- Depuração
                    interactionSucceeded = true
                    wait(0.5)
                end
            end

            if not interactionSucceeded then
                warn("[NPC Interaction] Nenhum RemoteEvent de interação funcional encontrado para '" .. npc.Name .. "'! A interação com o NPC pode falhar.")
            end
        else
            warn("[NPC Interaction] HumanoidRootPart do jogador não encontrada para interagir com NPC.")
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
    print("[runFarmCycle] Iniciando ciclo para: " .. questData.FarmName) -- Depuração
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
        InteractWithNPC(questNPCName, questCFrame)
        wait(1.5)
    elseif _G.AutoQuest then
        warn("[AutoQuest] Não foi possível interagir com o NPC da quest. Verifique NPCName e CFrameQuest.")
    else
        print("[AutoQuest] AutoQuest desativado. Pulando interação com NPC.") -- Depuração
    end

    -- 2. Teletransportar para o local de spawn dos monstros
    if mobName and mobSpawnCFrame then
        print("[Farm] Teletransportando para a área de farm de: " .. mobName)
        Teleport(mobSpawnCFrame)
        wait(1.5)

        local mobsKilled = 0
        local attemptCount = 0

        while mobsKilled < mobsToKill and _G.CurrentFarmActive and _G.FarmType == questData.FarmName and attemptCount < (mobsToKill * 20) do
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
                            print("[Farm] Jogador muito longe do alvo. Teletransportando para mais perto.") -- Depuração
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
                    warn("[Farm] Monstro '" .. mobName .. "' não derrotado ou problema no alvo. Buscando próximo. Saúde restante: " .. (_G.TargetMob.Humanoid.Health or "Desconhecido"))
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

    print("[runFarmCycle] Ciclo concluído para: " .. questData.FarmName) -- Depuração
    wait(2)
end

-- Função para iniciar qualquer farm
local function StartFarm(farmType, questData)
    print("[StartFarm] Tentando iniciar: " .. farmType) -- Depuração
    if _G.CurrentFarmActive then
        warn("[StartFarm] Já existe um auto farm ativo (" .. _G.FarmType .. "). Por favor, pare o farm atual antes de iniciar um novo.")
        return
    end

    local LocalPlayer = game:GetService("Players").LocalPlayer
    local playerLevel = 0
    if LocalPlayer and LocalPlayer:FindFirstChild("Data") and LocalPlayer.Data:FindFirstChild("Level") then
        playerLevel = LocalPlayer.Data.Level.Value
    else
        warn("[StartFarm] Não foi possível obter o nível do jogador. O objeto 'Data.Level' pode não estar disponível.")
        updateStatusText("Status: Erro (Sem Nível)")
        return
    end

    print("[StartFarm] Nível do jogador: " .. playerLevel .. ". Requer: " .. questData.MinLevel .. "-" .. questData.MaxLevel) -- Depuração

    if playerLevel < questData.MinLevel or playerLevel > questData.MaxLevel then
        print("[StartFarm] Nível do jogador (" .. playerLevel .. ") fora da faixa para " .. farmType .. " (" .. questData.MinLevel .. "-" .. questData.MaxLevel .. ").")
        print("[StartFarm] Por favor, selecione uma quest apropriada para seu nível.")
        updateStatusText("Status: Nível Incorreto")
        return -- Não inicia o farm se o nível estiver errado
    end

    _G.CurrentFarmActive = true
    _G.FarmType = farmType
    updateStatusText("Status: Ativo (" .. farmType .. ")")
    print("====================================")
    print("           " .. farmType .. " INICIADO!      ")
    print("====================================")

    autoFarmThread = spawn(function()
        print("[AutoFarmThread] Thread de farm iniciada para: " .. farmType) -- Depuração
        while _G.CurrentFarmActive and _G.FarmType == farmType do -- Garante que o loop só rode para o farm selecionado
            runFarmCycle(questData)
            if not _G.CurrentFarmActive then -- Verificação adicional para sair do loop se parado durante o ciclo
                break
            end
            wait(0.5) -- Pequeno delay para evitar sobrecarga
        end
        print("[AutoFarmThread] Loop de farm encerrado para: " .. farmType) -- Depuração
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
    print("[StopFarm] Chamado.") -- Depuração
    if not _G.CurrentFarmActive then
        print("Nenhum auto farm está ativo para parar.")
        return
    end
    print("[Controle] Sinal para parar Auto Farm enviado.")
    _G.CurrentFarmActive = false -- Isso vai encerrar o loop 'while _G.CurrentFarmActive'
    _G.TargetMob = nil
    -- A atualização do status e reset da thread será feita na própria thread do farm quando ela terminar
end

-- =====================================================================================================
--                            Bloco 3.5: Funções de Auto Click
-- =====================================================================================================

local function StartAutoClick()
    print("[StartAutoClick] Chamado.") -- Depuração
    if autoClickThread then
        print("[AutoClick] Auto Click já está em execução.")
        return
    end

    _G.AutoClick = true
    print("====================================")
    print("           Auto Click INICIADO!     ")
    print("====================================")

    autoClickThread = spawn(function()
        print("[AutoClickThread] Thread de Auto Click iniciada.") -- Depuração
        local mouse = game:GetService("Players").LocalPlayer:GetService("Mouse")
        while _G.AutoClick do
            -- print("[AutoClickThread] Clicando...") -- CUIDADO: pode gerar muitos logs
            mouse.Button1Down:fire()
            wait(_G.AutoClickDelay)
            mouse.Button1Up:fire()
            wait(_G.AutoClickDelay)
        end
        print("[AutoClickThread] Loop de Auto Click encerrado.") -- Depuração
        print("====================================")
        print("          Auto Click PARADO.        ")
        print("====================================")
        autoClickThread = nil
    end)
end

local function StopAutoClick()
    print("[StopAutoClick] Chamado.") -- Depuração
    _G.AutoClick = false
    print("[Controle] Sinal para parar Auto Click enviado.")
end

-- =====================================================================================================
--                            Bloco 4: Definições de Quests por Nível
-- =====================================================================================================

-- Definição de dados para cada quest/nível.
-- Crie uma tabela para cada quest, com todas as informações necessárias.
local QuestDefinitions = {
    -- PRIMEIRO MAR
    Bandit = {
        FarmName = "Bandit Farm", -- Nome para exibir na GUI e para identificar o farm
        MinLevel = 1,
        MaxLevel = 9,
        Mon = "Bandit",             -- Nome EXATO do modelo do mob no Workspace
        NameQuest = "Bandit Quest",
        NameMon = "Bandit",
        NPCName = "Bandit Quest Giver", -- Nome EXATO do modelo do NPC no Workspace
        MobsToKill = 5, -- Ou a quantidade que você quiser por ciclo
        -- SUBSTITUA ESTAS CFRAMES PELAS QUE VOCÊ OBTEVE NO JOGO!
        CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544),
        CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125)
    },
    Gorilla = {
        FarmName = "Gorilla Farm",
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
    -- ADICIONE MAIS QUESTS AQUI SEGUINDO O PADRÃO (copie e cole um bloco):
    -- Exemplo para o Segundo Mar:
    MarineCaptain = {
        FarmName = "Marine Captain Farm",
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
        FarmName = "Ice Admiral Farm",
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
    while wait() do
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
MainFrame.Size = UDim2.new(0, 250, 0, 420) -- Aumentado para acomodar mais botões e o auto click
MainFrame.Position = UDim2.new(0.5, -125, 0.5, -210) -- Centraliza o frame
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

-- Botão Toggle Auto Attack (afeta o farm de quests)
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
local Separator1 = Instance.new("Frame")
Separator1.Name = "Separator1"
Separator1.Size = UDim2.new(1, 0, 0, 2)
Separator1.Position = UDim2.new(0, 0, 0, 120)
Separator1.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
Separator1.Parent = MainFrame

-- Título para a seção de Auto Farm de Níveis
local LevelFarmLabel = Instance.new("TextLabel")
LevelFarmLabel.Name = "LevelFarmLabel"
LevelFarmLabel.Size = UDim2.new(1, 0, 0, 20)
LevelFarmLabel.Position = UDim2.new(0, 0, 0, 125)
LevelFarmLabel.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
LevelFarmLabel.Text = "--- Auto Farm por Nível ---"
LevelFarmLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
LevelFarmLabel.Font = Enum.Font.SourceSansBold
LevelFarmLabel.TextSize = 14
LevelFarmLabel.BorderSizePixel = 0
LevelFarmLabel.Parent = MainFrame

-- ScrollFrame para os botões de níveis
local LevelButtonsFrame = Instance.new("ScrollingFrame")
LevelButtonsFrame.Name = "LevelButtonsFrame"
LevelButtonsFrame.Size = UDim2.new(1, 0, 0.4, 0) -- Ajustado para caber o AutoClick
LevelButtonsFrame.Position = UDim2.new(0, 0, 0, 148) -- Abaixo do título de Level Farm
LevelButtonsFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
LevelButtonsFrame.BorderSizePixel = 0
LevelButtonsFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
LevelButtonsFrame.VerticalScrollBarInset = Enum.ScrollBarInset.Always
LevelButtonsFrame.ScrollingDirection = Enum.ScrollingDirection.Y
LevelButtonsFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Name = "QuestButtonLayout"
UIListLayout.Padding = UDim.new(0, 5)
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIListLayout.VerticalAlignment = Enum.VerticalAlignment.Top
UIListLayout.Parent = LevelButtonsFrame

-- Função para criar um botão de quest dinamicamente
local function createQuestButton(questName, questData)
    local button = Instance.new("TextButton")
    button.Name = questData.FarmName or questName .. "FarmButton"
    button.Size = UDim2.new(0.9, 0, 0, 30)
    button.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    button.Text = "Farm " .. questData.NameQuest .. " (Lvl " .. questData.MinLevel .. "-" .. questData.MaxLevel .. ")"
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 14
    button.BorderSizePixel = 0
    button.Parent = LevelButtonsFrame

    button.MouseButton1Click:Connect(function()
        print("[GUI Click] Botão " .. button.Name .. " clicado.") -- Depuração
        if _G.CurrentFarmActive then
            if _G.FarmType == questData.FarmName then
                StopFarm() -- Se já está ativo para esta quest, para
                print("[GUI] Parando " .. questData.FarmName .. ".")
            else
                warn("[GUI] Já existe um farm ativo (" .. _G.FarmType .. "). Por favor, pare-o primeiro.")
            end
        else
            StartFarm(questData.FarmName, questData)
        end
    end)
    return button
end

-- Adiciona os botões de quest dinamicamente
local currentYOffset = 0
local buttonHeight = 30
local padding = 5

local playerLevel = 0
if player and player:FindFirstChild("Data") and player.Data:FindFirstChild("Level") then
    playerLevel = player.Data.Level.Value
    print("Seu nível inicial detectado (para filtragem da GUI): " .. playerLevel)
else
    warn("Não foi possível obter o nível do jogador na inicialização da GUI.")
end


for questName, questData in pairs(QuestDefinitions) do
    -- Filtra as quests pelo mar atual do jogador E pelo nível
    local shouldDisplay = false
    if World1 and questData.MinLevel < 700 then
        -- No Primeiro Mar, mostramos todas as quests do Primeiro Mar
        shouldDisplay = true
    elseif World2 and questData.MinLevel >= 700 and questData.MinLevel < 1500 then
        -- No Segundo Mar, mostramos todas as quests do Segundo Mar
        shouldDisplay = true
    elseif World3 and questData.MinLevel >= 1500 then
        -- No Terceiro Mar, mostramos todas as quests do Terceiro Mar
        shouldDisplay = true
    end

    -- Adiciona uma condição adicional para mostrar apenas quests do seu nível aproximado
    -- Isso evita mostrar 200 botões e torna a GUI mais limpa para jogadores de nível alto.
    -- Vamos flexibilizar um pouco a faixa para mostrar mais opções ao invés de apenas a exata.
    if shouldDisplay and playerLevel ~= 0 then -- Apenas filtre se o nível foi lido com sucesso
        -- Mostra quests que começam até 50 níveis acima do seu nível atual
        -- E que terminam até 50 níveis abaixo do seu nível atual
        if not (questData.MinLevel > playerLevel + 50 or questData.MaxLevel < playerLevel - 50) then
             createQuestButton(questName, questData)
             currentYOffset = currentYOffset + buttonHeight + padding
        else
             print("Botão " .. questData.FarmName .. " filtrado (nível " .. playerLevel .. ") - Min:" .. questData.MinLevel .. " Max:" .. questData.MaxLevel)
        end
    elseif shouldDisplay and playerLevel == 0 then -- Se o nível não foi lido, mostre todas do mar correto (fallback)
        createQuestButton(questName, questData)
        currentYOffset = currentYOffset + buttonHeight + padding
    end
end
LevelButtonsFrame.CanvasSize = UDim2.new(0, 0, 0, currentYOffset)


-- Separador visual para Auto Click
local Separator2 = Instance.new("Frame")
Separator2.Name = "Separator2"
Separator2.Size = UDim2.new(1, 0, 0, 2)
Separator2.Position = UDim2.new(0, 0, 0, LevelButtonsFrame.Position.Y.Offset + LevelButtonsFrame.Size.Y.Offset + 5)
Separator2.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
Separator2.Parent = MainFrame

-- Título para a seção de Auto Click
local AutoClickLabel = Instance.new("TextLabel")
AutoClickLabel.Name = "AutoClickLabel"
AutoClickLabel.Size = UDim2.new(1, 0, 0, 20)
AutoClickLabel.Position = UDim2.new(0, 0, 0, Separator2.Position.Y.Offset + Separator2.Size.Y.Offset + 3)
AutoClickLabel.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
AutoClickLabel.Text = "--- Auto Click ---"
AutoClickLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoClickLabel.Font = Enum.Font.SourceSansBold
AutoClickLabel.TextSize = 14
AutoClickLabel.BorderSizePixel = 0
AutoClickLabel.Parent = MainFrame

-- Botão Toggle Auto Click
local ToggleAutoClickButton = Instance.new("TextButton")
ToggleAutoClickButton.Name = "AutoClick" -- Nome do botão para updateToggleButton
ToggleAutoClickButton.Size = UDim2.new(0.9, 0, 0, 25)
ToggleAutoClickButton.Position = UDim2.new(0.05, 0, 0, AutoClickLabel.Position.Y.Offset + AutoClickLabel.Size.Y.Offset + 5)
ToggleAutoClickButton.Font = Enum.Font.SourceSansBold
ToggleAutoClickButton.TextSize = 14
ToggleAutoClickButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleAutoClickButton.BackgroundColor3 = Color3.fromRGB(150, 70, 70) -- Começa OFF
ToggleAutoClickButton.Text = "AutoClick: OFF"
ToggleAutoClickButton.BorderSizePixel = 0
ToggleAutoClickButton.Parent = MainFrame

-- Adiciona um print para ter certeza que o event listener está sendo conectado
ToggleAutoClickButton.MouseButton1Click:Connect(function()
    print("[GUI Click] ---- Botão AutoClick CLICADO! ----") -- ESTE PRINT É CRÍTICO
    _G.AutoClick = not _G.AutoClick
    updateToggleButton(ToggleAutoClickButton, _G.AutoClick)
    if _G.AutoClick then
        StartAutoClick()
    else
        StopAutoClick()
    end
end)

-- Slider para ajuste de Delay do Auto Click
local DelaySliderLabel = Instance.new("TextLabel")
DelaySliderLabel.Name = "DelaySliderLabel"
DelaySliderLabel.Size = UDim2.new(0.9, 0, 0, 15)
DelaySliderLabel.Position = UDim2.new(0.05, 0, 0, ToggleAutoClickButton.Position.Y.Offset + ToggleAutoClickButton.Size.Y.Offset + 5)
DelaySliderLabel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
DelaySliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
DelaySliderLabel.Font = Enum.Font.SourceSans
DelaySliderLabel.TextSize = 12
DelaySliderLabel.TextXAlignment = Enum.TextXAlignment.Left
DelaySliderLabel.Text = "Delay: " .. string.format("%.2f", _G.AutoClickDelay) .. "s (Min: 0.01)"
DelaySliderLabel.Parent = MainFrame

local DelaySlider = Instance.new("Frame")
DelaySlider.Name = "DelaySlider"
DelaySlider.Size = UDim2.new(0.9, 0, 0, 15)
DelaySlider.Position = UDim2.new(0.05, 0, 0, DelaySliderLabel.Position.Y.Offset + DelaySliderLabel.Size.Y.Offset)
DelaySlider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
DelaySlider.BorderSizePixel = 0
DelaySlider.Parent = MainFrame

local SliderButton = Instance.new("TextButton")
SliderButton.Name = "SliderButton"
SliderButton.Size = UDim2.new(0, 20, 1, 0)
SliderButton.Position = UDim2.new(0, 0, 0, 0)
SliderButton.BackgroundColor3 = Color3.fromRGB(100, 100, 200) -- Azul para o "botão" do slider
SliderButton.Text = ""
SliderButton.BorderSizePixel = 0
SliderButton.Draggable = true -- Permite arrastar o botão do slider
SliderButton.Parent = DelaySlider

local minDelay = 0.01
local maxDelay = 1.0

SliderButton.Changed:Connect(function(property)
    if property == "Position" then
        local ratio = SliderButton.Position.X.Offset / (DelaySlider.AbsoluteSize.X - SliderButton.AbsoluteSize.X)
        _G.AutoClickDelay = minDelay + (maxDelay - minDelay) * ratio
        DelaySliderLabel.Text = "Delay: " .. string.format("%.2f", _G.AutoClickDelay) .. "s (Min: 0.01)"
    end
end)


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
    print("[GUI Click] Botão Stop All Farm clicado.") -- Depuração
    StopFarm()
    StopAutoClick() -- Garante que o auto click também pare
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
    StopAutoClick()
    ScreenGui:Destroy()
    warn("Script de Auto Farm finalizado e GUI destruída.")
end)

-- Inicializa o status na GUI
updateStatusText("Status: Inativo")

-- Mensagem de inicialização
print("Auto Farm GUI carregada. Selecione uma opção de farm ou use o Auto Click.")
