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
        if v:IsA("Model") and v.Name:find(name, 1, true) and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 and v:FindFirstChild("HumanoidRootPart") then
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

-- Função para atacar um alvo (usando RemoteEvents para interação com o servidor)
function AttackTarget(target)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    if not (PlayerHRP and target and target:FindFirstChild("HumanoidRootPart") and target:FindFirstChild("Humanoid") and target.Humanoid.Health > 0) then
        return
    end

    PlayerHRP.CFrame = CFrame.new(PlayerHRP.Position, target.HumanoidRootPart.Position)

    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local AttackRemote = ReplicatedStorage:FindFirstChild("Remote")
    if not AttackRemote then AttackRemote = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Attack") end
    if not AttackRemote then AttackRemote = ReplicatedStorage:FindFirstChild("CombatEvents") and ReplicatedStorage.CombatEvents:FindFirstChild("Damage") end

    if AttackRemote and AttackRemote:IsA("RemoteEvent") then
        AttackRemote:FireServer("Attack", target.HumanoidRootPart)
        print("[Attack] Tentando atacar via RemoteEvent: " .. target.Name)
    else
        warn("[Attack] Remote de ataque não encontrado ou inválido! Atacando via clique do mouse (pode não dar dano).")
        game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Down:fire()
        wait(0.1)
        game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Up:fire()
    end
    wait(0.2)
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
                Mon = "Bandit",
                LevelQuest = 1,
                NameQuest = "Bandit Quest",
                NameMon = "Bandit",
                NPCName = "Bandit Quest Giver",
                CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544),
                CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125)
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
        end
    elseif World2 then
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
        end
    elseif World3 then
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
            local InteractRemote = ReplicatedStorage:FindFirstChild("Remote")
            if not InteractRemote then InteractRemote = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Interact") end
            if not InteractRemote then InteractRemote = ReplicatedStorage:FindFirstChild("QuestHandler") and ReplicatedStorage.QuestHandler:FindFirstChild("AcceptQuest") end

            if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                InteractRemote:FireServer("Quest", npc.Name)
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

local autoFarmThread = nil

function StartAutoFarm()
    if _G.AutoFarm and autoFarmThread then
        print("[AutoFarm] Auto Farm já está em execução.")
        return
    end

    _G.AutoFarm = true
    print("====================================")
    print("           Auto Farm INICIADO!      ")
    print("====================================")
    updateStatusText("Status: Ativo")

    autoFarmThread = spawn(function()
        while _G.AutoFarm do
            local questData = CheckQuest()

            -- Removido a interrupção abrupta se faltar algum dado.
            -- Agora, ele só avisará e continuará tentando, mas se as CFrames ou nomes estiverem errados,
            -- as funções de teletransporte e encontrar mobs/NPCs falharão.
            if not questData.Mon then warn("Quest data missing 'Mon'. Check CheckQuest().") end
            if not questData.NPCName then warn("Quest data missing 'NPCName'. Check CheckQuest().") end
            if not questData.CFrameQuest then warn("Quest data missing 'CFrameQuest'. Check CheckQuest().") end
            if not questData.CFrameMon then warn("Quest data missing 'CFrameMon'. Check CheckQuest().") end

            local questNPCName = questData.NPCName
            local mobName = questData.Mon
            local questCFrame = questData.CFrameQuest
            local mobSpawnCFrame = questData.CFrameMon
            local mobsToKill = 5

            print("\n--- Ciclo de Farm para Nível " .. game:GetService("Players").LocalPlayer.Data.Level.Value .. " (Quest: " .. (questData.NameQuest or "Desconhecida") .. ") ---")

            if _G.AutoQuest and questNPCName and questCFrame then
                print("[AutoQuest] Teletransportando para o NPC da Quest: " .. questNPCName)
                Teleport(questCFrame)
                wait(1.5)
                InteractWithNPC(questNPCName)
                wait(1.5)
            elseif _G.AutoQuest then
                warn("[AutoQuest] Não foi possível interagir com o NPC da quest. Verifique NPCName e CFrameQuest.")
            end

            if mobName and mobSpawnCFrame then
                print("[Farm] Teletransportando para a área de farm de: " .. mobName)
                Teleport(mobSpawnCFrame)
                wait(1.5)

                local mobsKilled = 0
                local attemptCount = 0

                while mobsKilled < mobsToKill and _G.AutoFarm and attemptCount < (mobsToKill * 20) do
                    _G.TargetMob = FindMob(mobName)

                    if _G.TargetMob then
                        print("[Farm] Atacando: " .. _G.TargetMob.Name .. " (" .. mobsKilled .. "/" .. mobsToKill .. " abatidos)")
                        if _G.AutoAttack then
                            AttackTarget(_G.TargetMob)
                        end
                        
                        local attackAttempts = 0
                        local LocalPlayer = game:GetService("Players").LocalPlayer
                        while _G.TargetMob and _G.TargetMob:FindFirstChild("Humanoid") and _G.TargetMob.Humanoid.Health > 0 and attackAttempts < 50 and _G.AutoFarm do
                            if _G.AutoAttack then
                                AttackTarget(_G.TargetMob)
                            end
                            wait(0.2)
                            attackAttempts = attackAttempts + 1
                            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and _G.TargetMob:FindFirstChild("HumanoidRootPart") and (LocalPlayer.Character.HumanoidRootPart.Position - _G.TargetMob.HumanoidRootPart.Position).magnitude > 20 then
                                Teleport(_G.TargetMob.HumanoidRootPart.CFrame * CFrame.new(0,0,5))
                                wait(0.5)
                            end
                        end

                        if not _G.TargetMob or not _G.TargetMob:FindFirstChild("Humanoid") or _G.TargetMob.Humanoid.Health <= 0 then
                            print("[Farm] Monstro derrotado: " .. mobName)
                            mobsKilled = mobsKilled + 1
                            _G.TargetMob = nil
                            wait(0.5)
                        else
                            warn("[Farm] Monstro não derrotado ou problema no alvo. Buscando próximo. Saúde restante: " .. _G.TargetMob.Humanoid.Health)
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
                warn("[Farm] Não foi possível farmar. Verifique o nome do monstro e a CFrame de spawn em CheckQuest().")
            end


            if _G.AutoQuest and questNPCName and questCFrame then
                print("[AutoQuest] Voltando para o NPC da Quest para entregar: " .. questNPCName)
                Teleport(questCFrame)
                wait(1.5)
                InteractWithNPC(questNPCName)
                wait(1.5)
            elseif _G.AutoQuest then
                warn("[AutoQuest] Não foi possível entregar a quest. Verifique NPCName e CFrameQuest.")
            end

            wait(2)
        end
        print("====================================")
        print("          Auto Farm PARADO.         ")
        print("====================================")
        updateStatusText("Status: Inativo")
        autoFarmThread = nil
    end)
end

function StopAutoFarm()
    _G.AutoFarm = false
    _G.TargetMob = nil
    print("[Controle] Sinal para parar Auto Farm enviado.")
    updateStatusText("Status: Inativo")
end

-- --- THREADS E LISTENERS SECUNDÁRIOS ---

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
local coreGui = game:GetService("CoreGui")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AutoFarmGUI"
ScreenGui.Parent = coreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 220, 0, 180)
MainFrame.Position = UDim2.new(0.5, -110, 0.5, -90)
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
        button.BackgroundColor3 = Color3.fromRGB(70, 150, 70)
    else
        button.Text = button.Name .. ": OFF"
        button.BackgroundColor3 = Color3.fromRGB(150, 70, 70)
    end
end

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
    StopAutoFarm()
    ScreenGui:Destroy()
    warn("Script de Auto Farm finalizado e GUI destruída.")
end)

updateStatusText("Status: Inativo")
