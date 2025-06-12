-- Verifica o PlaceId para determinar o mundo
if game.PlaceId == 2753915549 then
    World1 = true
elseif game.PlaceId == 4442272183 then
    World2 = true
elseif game.PlaceId == 7449423635 then
    World3 = true
else
    game:GetService("Players").LocalPlayer:Kick("Do not Support, Please wait...")
end

-- Variáveis globais (_G)
_G.Remove_Effect = true -- Para remover o efeito de morte, se ainda existir
_G.AutoFarm = false -- Variável para controlar o auto-farm
_G.AutoQuest = false -- Variável para controlar a aceitação/entrega de quests
_G.AutoAttack = true -- Variável para controlar o ataque automático (pode desativar se quiser apenas teletransporte)
_G.TargetMob = nil -- O monstro alvo atual

-- --- FUNÇÕES AUXILIARES GERAIS ---

-- Placeholder para a função Hop(). Seu executor pode já ter isso ou você pode precisar de uma implementação.
-- Normalmente, a função Hop() em exploits serve para trocar de servidor.
function Hop()
    -- Esta é uma implementação SIMPLES de Hop para mudar de servidor.
    -- Seu executor pode ter uma função nativa melhor, como 'teleport.toRandomServer()'.
    -- Ou você pode querer uma função que te teletransporte para um servidor vazio ou com menos players.
    print("Tentando trocar de servidor...")
    local TeleportService = game:GetService("TeleportService")
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer

    -- Tenta teletransportar para um servidor diferente do mesmo PlaceId.
    TeleportService:Teleport(game.PlaceId, LocalPlayer)
end

-- Função para teletransportar o jogador
function Teleport(cframe)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    if LocalPlayer and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = cframe
        print("Teletransportado para: " .. cframe.Position.X .. ", " .. cframe.Position.Y .. ", " .. cframe.Position.Z)
    end
end

-- Função para encontrar um monstro pelo nome
function FindMob(name)
    local workspace = game:GetService("Workspace")
    for i, v in pairs(workspace:GetChildren()) do
        if v:IsA("Model") and v.Name:find(name, 1, true) and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
            return v
        end
    end
    return nil
end

-- Função para atacar um alvo (simula cliques ou interações)
function AttackTarget(target)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    if LocalPlayer and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and target and target:FindFirstChild("HumanoidRootPart") and target.Humanoid.Health > 0 then
        -- Simula um ataque básico (clique). Em alguns jogos, isso pode ser o suficiente.
        -- Para jogos mais complexos, você pode precisar de 'FireServer' em remotes de ataque.
        -- Exemplo: game:GetService("ReplicatedStorage").Events.Attack:FireServer(target.HumanoidRootPart)
        -- Para Blox Fruits, a forma de atacar pode ser mais complexa (habilidades, M1, etc.)
        -- Esta é uma abordagem genérica que pode funcionar para alguns jogos.
        LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(LocalPlayer.Character.HumanoidRootPart.Position, target.HumanoidRootPart.Position)
        game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Down:fire()
        wait(0.1) -- Pequeno delay para simular o ataque
        game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Up:fire()
    end
end

-- --- LÓGICA DO JOGO ESPECÍFICA (CheckQuest agora define dados da quest) ---

function CheckQuest()
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local MyLevel = LocalPlayer.Data.Level.Value
    local questData = {}

    if World1 then
        if MyLevel >= 1 and MyLevel <= 9 then
            questData = {
                Mon = "Bandit",
                LevelQuest = 1,
                NameQuest = "BanditQuest1",
                NameMon = "Bandit",
                CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544),
                CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125),
                NPCName = "Bandit Quest Giver" -- Nome do NPC que dá a quest
            }
        elseif MyLevel >= 10 and MyLevel <= 19 then
            questData = {
                Mon = "Gorilla",
                LevelQuest = 10,
                NameQuest = "GorillaQuest1",
                NameMon = "Gorilla",
                CFrameQuest = CFrame.new(1471.74866, 15.4495068, 1618.173, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544),
                CFrameMon = CFrame.new(1495.60266, 27.00250816345215, 1632.74878),
                NPCName = "Gorilla Quest Giver"
            }
        -- ... adicione mais quests para World1, World2, World3 conforme o jogo ...
        elseif MyLevel >= 20 and MyLevel <= 29 then
            questData = {
                Mon = "Jungle Pirate",
                LevelQuest = 20,
                NameQuest = "Jungle Pirate Quest",
                NameMon = "Jungle Pirate",
                CFrameQuest = CFrame.new(1471.74866, 15.4495068, 1618.173, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544), -- Exemplo, ajuste as CFrames
                CFrameMon = CFrame.new(1500, 27, 1650), -- Exemplo, ajuste as CFrames
                NPCName = "Jungle Pirate Quest Giver"
            }
        end
    elseif World2 then
        -- Exemplo para World2
        if MyLevel >= 700 and MyLevel <= 749 then
            questData = {
                Mon = "Marine Captain",
                LevelQuest = 700,
                NameQuest = "Marine Captain Quest",
                NameMon = "Marine Captain",
                CFrameQuest = CFrame.new(-338.000, 31.000, 680.000), -- Exemplo
                CFrameMon = CFrame.new(-400.000, 31.000, 700.000), -- Exemplo
                NPCName = "Marine Quest Giver"
            }
        -- Adicione mais quests para World2
        end
    elseif World3 then
        -- Adicione quests para World3
        if MyLevel >= 1500 and MyLevel <= 1549 then
            questData = {
                Mon = "Awakened Ice Admiral",
                LevelQuest = 1500,
                NameQuest = "Ice Admiral Quest",
                NameMon = "Ice Admiral",
                CFrameQuest = CFrame.new(200.000, 100.000, 300.000), -- Exemplo
                CFrameMon = CFrame.new(250.000, 100.000, 350.000), -- Exemplo
                NPCName = "Ice Admiral Quest Giver"
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
            -- Teleporta para perto do NPC
            Teleport(npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 5)) -- Ajusta para ficar na frente do NPC

            wait(0.5) -- Espera um pouco

            -- Tenta interagir (pode ser um RemoteEvent para alguns jogos)
            -- Para Blox Fruits, geralmente é um remote chamado 'Remote' ou 'Interact'
            local Remote = game:GetService("ReplicatedStorage"):FindFirstChild("Remote") or game:GetService("ReplicatedStorage"):FindFirstChild("Events") and game:GetService("ReplicatedStorage").Events:FindFirstChild("Interact")

            if Remote and Remote:IsA("RemoteEvent") then
                -- Argumentos específicos para a interação com o NPC de quest em Blox Fruits
                -- Estes argumentos podem variar dependendo da atualização do jogo.
                -- Você pode precisar usar um sniffer de pacotes (ex: Wireshark com um executor) para encontrar os argumentos exatos.
                -- Exemplo comum para aceitar quest:
                Remote:FireServer("Quest", npc.Name) -- Tenta interagir para aceitar/completar a quest
                print("Tentando interagir com NPC: " .. npc.Name)
            else
                warn("Remote de interação não encontrado ou inválido!")
                -- Se não houver Remote, talvez um clique no NPC funcione para alguns jogos
                -- Ou simular o E para interagir
            end
            wait(0.5) -- Pequeno delay após a interação
        end
    end
end

-- --- FUNÇÃO PRINCIPAL DE AUTO-FARM ---

function StartAutoFarm()
    _G.AutoFarm = true
    print("Auto Farm INICIADO!")

    while _G.AutoFarm do
        local questData = CheckQuest()

        if not questData.Mon then
            print("Nenhuma quest encontrada para o seu nível atual. Parando Auto Farm.")
            _G.AutoFarm = false
            break
        end

        local questNPCName = questData.NPCName
        local mobName = questData.Mon
        local questCFrame = questData.CFrameQuest
        local mobSpawnCFrame = questData.CFrameMon

        -- 1. Aceitar/Completar Quest (se _G.AutoQuest estiver ativado)
        if _G.AutoQuest and questNPCName and questCFrame then
            print("Teletransportando para o NPC da Quest: " .. questNPCName)
            Teleport(questCFrame)
            wait(1)
            InteractWithNPC(questNPCName)
            wait(2) -- Espera um pouco após interagir com o NPC
        end

        -- 2. Teletransportar para o local de spawn dos monstros
        print("Teletransportando para a área de farm de: " .. mobName)
        Teleport(mobSpawnCFrame)
        wait(1) -- Espera um pouco para o carregamento do mapa/mobs

        -- 3. Encontrar e atacar monstros
        local mobsKilled = 0
        local targetMobCount = 5 -- Quantidade de mobs para matar por quest (ajuste conforme o jogo)

        while mobsKilled < targetMobCount and _G.AutoFarm do
            _G.TargetMob = FindMob(mobName)

            if _G.TargetMob then
                print("Atacando: " .. _G.TargetMob.Name)
                if _G.AutoAttack then
                    AttackTarget(_G.TargetMob)
                end
                
                -- Espera o monstro morrer ou tenta atacar novamente
                local attackAttempts = 0
                while _G.TargetMob and _G.TargetMob:FindFirstChild("Humanoid") and _G.TargetMob.Humanoid.Health > 0 and attackAttempts < 20 do
                    if _G.AutoAttack then
                        AttackTarget(_G.TargetMob)
                    end
                    wait(0.5) -- Espera para o ataque registrar
                    attackAttempts = attackAttempts + 1
                end

                if _G.TargetMob and (_G.TargetMob.Humanoid.Health <= 0 or not _G.TargetMob:FindFirstChild("Humanoid")) then
                    print("Monstro derrotado: " .. mobName)
                    mobsKilled = mobsKilled + 1
                    _G.TargetMob = nil -- Limpa o alvo
                else
                    warn("Monstro não derrotado ou problema no alvo. Buscando próximo.")
                    _G.TargetMob = nil -- Limpa o alvo para buscar outro
                end
            else
                print("Nenhum monstro '" .. mobName .. "' encontrado. Esperando o spawn ou procurando novamente...")
                wait(2) -- Espera um pouco antes de tentar encontrar novamente
            end
            wait(0.1) -- Pequeno delay entre as iterações do loop interno
        end

        print("Mobs suficientes derrotados para a quest de " .. mobName .. ".")

        -- 4. Voltar para o NPC da Quest para entregar (se _G.AutoQuest estiver ativado)
        if _G.AutoQuest and questNPCName and questCFrame then
            print("Voltando para o NPC da Quest para entregar: " .. questNPCName)
            Teleport(questCFrame)
            wait(1)
            InteractWithNPC(questNPCName)
            wait(2) -- Espera um pouco após entregar a quest
        end

        wait(1) -- Pequeno delay antes de checar a próxima quest ou loop
    end
    print("Auto Farm PARADO.")
end

function StopAutoFarm()
    _G.AutoFarm = false
    _G.TargetMob = nil
    print("Auto Farm Desativado.")
end

-- --- THREADS E LISTENERS SECUNDÁRIOS ---

-- Thread para remover efeitos de morte (original do seu script)
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

-- Thread para detectar administradores/jogadores específicos e dar Hop (original do seu script)
-- CUIDADO: Esta função 'Hop()' precisa ser funcional no seu executor.
spawn(function()
    while wait() do
        for i,v in pairs(game.Players:GetPlayers()) do
            if v.Name == "red_game43" or v.Name == "rip_indra" or v.Name == "Axiore" or v.Name == "Polkster" or v.Name == "wenlocktoad" or v.Name == "Daigrock" or v.Name == "toilamvidamme" or v.Name == "oofficialnoobie" or v.Name == "Uzoth" or v.Name == "Azarth" or v.Name == "arlthmetic" or v.Name == "Death_King" or v.Name == "Lunoven" or v.Name == "TheGreateAced" or v.Name == "rip_fud" or v.Name == "drip_mama" or v.Name == "layandikit12" or v.Name == "Hingoi" then
                print("Admin/Player específico detectado: " .. v.Name .. ". Dando Hop!")
                Hop() -- Chama a função Hop
                wait(10) -- Espera um pouco após o hop para não loopar
            end
        end
    end
end)

-- Anti-AFK (para evitar ser chutado por inatividade)
spawn(function()
    while wait(10) do
        if _G.AutoFarm then
            local LocalPlayer = game:GetService("Players").LocalPlayer
            local Humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
            if Humanoid then
                Humanoid:ChangeState(Enum.HumanoidStateType.Running) -- Simula movimento
                wait(0.1)
                Humanoid:ChangeState(Enum.HumanoidStateType.Idle)
            end
        end
    end
end)


-- --- INTERFACE DE CONTROLE (OPCIONAL: Para usar via console do executor) ---
-- Você pode chamar StartAutoFarm() ou StopAutoFarm() no console do seu executor.

-- Exemplo de como usar (se você quiser um sistema de comando básico via chat ou console):
-- Para iniciar: _G.AutoFarm = true; StartAutoFarm()
-- Para parar: _G.AutoFarm = false; StopAutoFarm()
-- Para ativar/desativar auto-quest: _G.AutoQuest = true ou _G.AutoQuest = false
-- Para ativar/desativar auto-attack: _G.AutoAttack = true ou _G.AutoAttack = false

-- Sugestão: Crie um UI para controlar isso facilmente (requer mais código)
-- Ex: Adicione um botão no seu executor para executar StartAutoFarm()
-- Ex: Adicione um comando de chat no seu executor:
-- loadstring(game:HttpGet("https://raw.githubusercontent.com/seuusuario/seurepositorio/main/autoscript.lua"))()
-- Mas para isso, o script precisaria ser hospedado online.

-- --- INÍCIO AUTOMÁTICO (Descomente se quiser que o script comece automaticamente ao injetar) ---
-- warn("Script de Auto Farm injetado! Ative _G.AutoFarm = true; para iniciar ou chame StartAutoFarm().")
-- Para começar o farm automaticamente:
-- _G.AutoFarm = true
-- _G.AutoQuest = true -- Defina como false se não quiser que ele pegue/entregue quests
-- _G.AutoAttack = true -- Defina como false se quiser apenas teletransporte
-- spawn(StartAutoFarm)
