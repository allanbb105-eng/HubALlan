-- Verifica o PlaceId para determinar o mundo
local World1 = false
local World2 = false
local World3 = false

if game.PlaceId == 2753915549 then -- ID do Primeiro Mar (First Sea)
    World1 = true
elseif game.PlaceId == 4442272183 then -- ID do Segundo Mar (Second Sea)
    World2 = true
elseif game.PlaceId == 7449423635 then -- ID do Terceiro Mar (Third Sea)
    World3 = true
else
    game:GetService("Players").LocalPlayer:Kick("Local não suportado. Por favor, entre em um dos mares suportados.")
end

-- Variáveis globais (_G) para controle do script
_G.Remove_Effect = true -- Para remover o efeito de morte (original do seu script)
_G.AutoFarm = false     -- Controla se o auto-farm está ativo (true para ligar, false para desligar)
_G.AutoQuest = true     -- Controla se o script deve aceitar e entregar quests automaticamente
_G.AutoAttack = true    -- Controla se o script deve atacar monstros automaticamente
_G.TargetMob = nil      -- Referência ao monstro alvo atual

-- --- FUNÇÕES AUXILIARES GERAIS ---

-- Placeholder para a função Hop(). Seu executor pode já ter isso ou você pode precisar de uma implementação mais robusta.
-- Esta é uma implementação SIMPLES que tenta teletransportar para o mesmo PlaceId.
-- Muitos executores oferecem funções mais avançadas como `teleport.toRandomServer()` ou `teleport.toEmptyServer()`.
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

-- Função para encontrar um monstro pelo nome
function FindMob(name)
    local workspace = game:GetService("Workspace")
    local foundMob = nil
    local shortestDistance = math.huge
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    if not PlayerHRP then return nil end

    for i, v in pairs(workspace:GetChildren()) do
        -- Verifica se é um modelo, tem o nome correto, tem um Humanoid e está vivo
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

-- Função para atacar um alvo (simula cliques ou interações com RemoteEvents)
function AttackTarget(target)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    if not (PlayerHRP and target and target:FindFirstChild("HumanoidRootPart") and target:FindFirstChild("Humanoid") and target.Humanoid.Health > 0) then
        return
    end

    -- Tenta atacar via RemoteEvent (comum em Blox Fruits)
    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local AttackRemote = ReplicatedStorage:FindFirstChild("Remote") -- Nome comum para o Remote de ataque em Blox Fruits

    if AttackRemote and AttackRemote:IsA("RemoteEvent") then
        -- Os argumentos do FireServer podem variar muito dependendo da atualização do jogo.
        -- Para M1 (ataque básico), geralmente basta o alvo ou a CFrame do alvo.
        -- Você pode precisar de um Remote Spy para verificar os argumentos exatos.
        AttackRemote:FireServer("Attack", target.HumanoidRootPart) -- Exemplo comum de chamada para ataque
        -- Para habilidades específicas, você pode precisar de outro Remote ou de argumentos diferentes:
        -- AttackRemote:FireServer("Ability", "FlamePillar", target.HumanoidRootPart)
        -- print("[Attack] Atacando via RemoteEvent: " .. target.Name)
    else
        -- Fallback: Simula um clique do mouse se o RemoteEvent não for encontrado ou não for usado.
        -- Move o jogador para olhar para o alvo antes de clicar.
        PlayerHRP.CFrame = CFrame.new(PlayerHRP.Position, target.HumanoidRootPart.Position)
        game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Down:fire()
        wait(0.1) -- Pequeno delay para simular o ataque
        game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Up:fire()
        -- print("[Attack] Atacando via clique do mouse (fallback): " .. target.Name)
    end
end

-- --- LÓGICA DO JOGO ESPECÍFICA (CheckQuest agora retorna uma tabela com dados da quest) ---

function CheckQuest()
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local MyLevel = LocalPlayer.Data.Level.Value
    local questData = {}

    if World1 then
        if MyLevel >= 1 and MyLevel <= 9 then
            questData = {
                Mon = "Bandit", -- Nome do modelo do monstro
                LevelQuest = 1,
                NameQuest = "Bandit Quest", -- Nome da quest no jogo
                NameMon = "Bandit", -- Nome para encontrar o monstro (geralmente o mesmo que 'Mon')
                NPCName = "Bandit Quest Giver", -- Nome do NPC que dá a quest
                CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544), -- CFrame do NPC da quest
                CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125) -- CFrame da área de spawn dos monstros
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
        -- [[ ADICIONE MAIS QUESTS AQUI PARA WORLD1 ]]
        -- Exemplo para o próximo conjunto de níveis em World1:
        elseif MyLevel >= 20 and MyLevel <= 29 then
            questData = {
                Mon = "Jungle Pirate", -- Ajuste o nome do monstro conforme o jogo
                LevelQuest = 20,
                NameQuest = "Jungle Pirate Quest",
                NameMon = "Jungle Pirate",
                NPCName = "Jungle Pirate Quest Giver", -- Ajuste o nome do NPC
                CFrameQuest = CFrame.new(0,0,0), -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
                CFrameMon = CFrame.new(0,0,0) -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
            }
        end
    elseif World2 then
        -- [[ ADICIONE QUESTS AQUI PARA WORLD2 ]]
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
        -- Adicione mais quests para World2
        end
    elseif World3 then
        -- [[ ADICIONE QUESTS AQUI PARA WORLD3 ]]
        -- Exemplo para World3:
        if MyLevel >= 1500 and MyLevel <= 1549 then
            questData = {
                Mon = "Ice Admiral", -- Nome do modelo do monstro no jogo
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
            -- Teleporta para perto do NPC
            Teleport(npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 5)) -- Ajusta para ficar na frente do NPC
            wait(0.5) -- Espera um pouco

            -- Tenta interagir (pode ser um RemoteEvent para alguns jogos, como Blox Fruits)
            local ReplicatedStorage = game:GetService("ReplicatedStorage")
            local InteractRemote = ReplicatedStorage:FindFirstChild("Remote") or ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Interact")

            if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                -- Os argumentos para interagir com NPC de quest em Blox Fruits podem ser específicos.
                -- Use um Remote Spy para verificar os argumentos exatos (ex: "Quest", NPC.Name, quest.Name).
                InteractRemote:FireServer("Quest", npc.Name) -- Tentativa comum para Blox Fruits
                print("[NPC Interaction] Tentando interagir com NPC: " .. npc.Name)
            else
                warn("[NPC Interaction] Remote de interação não encontrado ou inválido! Tentando fallback...")
                -- Fallback: Simular um clique ou pressionar 'E' se não houver um Remote direto
                -- (Isso raramente funciona para sistemas de quest complexos)
            end
            wait(0.5) -- Pequeno delay após a interação
        end
    else
        warn("[NPC Interaction] NPC '" .. npcName .. "' não encontrado para interação.")
    end
end

-- --- FUNÇÃO PRINCIPAL DE AUTO-FARM ---

function StartAutoFarm()
    _G.AutoFarm = true
    print("====================================")
    print("           Auto Farm INICIADO!      ")
    print("====================================")
    print("Controles: ")
    print(" _G.AutoFarm = false   (Para parar)")
    print(" _G.G.AutoQuest = false (Desativa auto-quest)")
    print(" _G.AutoAttack = false (Desativa auto-ataque)")
    print("====================================")

    while _G.AutoFarm do
        local questData = CheckQuest()

        if not questData.Mon then
            warn("Nenhuma quest encontrada para o seu nível atual. Verifique a função CheckQuest(). Parando Auto Farm.")
            _G.AutoFarm = false
            break
        end

        local questNPCName = questData.NPCName
        local mobName = questData.Mon
        local questCFrame = questData.CFrameQuest
        local mobSpawnCFrame = questData.CFrameMon
        local mobsToKill = 5 -- Quantidade de mobs para matar por quest (AJUSTE CONFORME O JOGO)

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
        local attemptCount = 0 -- Contador de tentativas para evitar loops infinitos

        while mobsKilled < mobsToKill and _G.AutoFarm and attemptCount < (mobsToKill * 10) do -- Limita tentativas
            _G.TargetMob = FindMob(mobName)

            if _G.TargetMob then
                print("[Farm] Atacando: " .. _G.TargetMob.Name .. " (" .. mobsKilled .. "/" .. mobsToKill .. " abatidos)")
                if _G.AutoAttack then
                    AttackTarget(_G.TargetMob)
                end
                
                -- Espera o monstro morrer ou tenta atacar novamente
                local attackAttempts = 0
                while _G.TargetMob and _G.TargetMob:FindFirstChild("Humanoid") and _G.TargetMob.Humanoid.Health > 0 and attackAttempts < 30 and _G.AutoFarm do
                    if _G.AutoAttack then
                        AttackTarget(_G.TargetMob)
                    end
                    wait(0.2) -- Pequeno delay entre ataques
                    attackAttempts = attackAttempts + 1
                    -- Se o alvo estiver muito longe após um teleporte, tenta se reaproximar
                    if (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - _G.TargetMob.HumanoidRootPart.Position).magnitude > 20 then
                        Teleport(_G.TargetMob.HumanoidRootPart.CFrame * CFrame.new(0,0,5)) -- Teleporta para perto do alvo
                    end
                end

                if not _G.TargetMob or not _G.TargetMob:FindFirstChild("Humanoid") or _G.TargetMob.Humanoid.Health <= 0 then
                    print("[Farm] Monstro derrotado: " .. mobName)
                    mobsKilled = mobsKilled + 1
                    _G.TargetMob = nil -- Limpa o alvo
                    wait(0.5) -- Pequeno delay antes de procurar o próximo
                else
                    warn("[Farm] Monstro não derrotado ou problema no alvo. Buscando próximo. Saúde restante: " .. _G.TargetMob.Humanoid.Health)
                    _G.TargetMob = nil -- Limpa o alvo para buscar outro
                end
            else
                print("[Farm] Nenhum monstro '" .. mobName .. "' encontrado. Esperando o spawn ou procurando novamente...")
                wait(2) -- Espera um pouco antes de tentar encontrar novamente
            end
            attemptCount = attemptCount + 1
            wait(0.1) -- Pequeno delay entre as iterações do loop interno
        end

        if mobsKilled >= mobsToKill then
            print("[Farm] Mobs suficientes derrotados (" .. mobsKilled .. "/" .. mobsToKill .. ") para a quest de " .. mobName .. ".")
        else
            warn("[Farm] Não foi possível derrotar todos os mobs. Total: " .. mobsKilled .. ". Verifique se as CFrames e os nomes dos mobs estão corretos.")
        end


        -- 4. Voltar para o NPC da Quest para entregar (se _G.AutoQuest estiver ativado)
        if _G.AutoQuest and questNPCName and questCFrame then
            print("[AutoQuest] Voltando para o NPC da Quest para entregar: " .. questNPCName)
            Teleport(questCFrame)
            wait(1.5)
            InteractWithNPC(questNPCName)
            wait(1.5) -- Espera após entregar a quest
        end

        wait(2) -- Pequeno delay antes de checar a próxima quest ou loop
    end
    print("====================================")
    print("          Auto Farm PARADO.         ")
    print("====================================")
end

function StopAutoFarm()
    _G.AutoFarm = false
    _G.TargetMob = nil
    print("[Controle] Sinal para parar Auto Farm enviado.")
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
                -- Simula um movimento mínimo
                Humanoid:ChangeState(Enum.HumanoidStateType.Running)
                wait(0.1)
                Humanoid:ChangeState(Enum.HumanoidStateType.Idle)
                -- Ou pode ser uma pequena rotação do personagem
                -- LocalPlayer.Character.HumanoidRootPart.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(1), 0)
            end
        end
    end
end)


-- --- INSTRUÇÕES PARA INICIAR O AUTO FARM ---
warn("Script de Auto Farm injetado! Para iniciar, digite no console do seu executor:")
warn("   _G.AutoFarm = true")
warn("   spawn(StartAutoFarm)")
warn("\nPara parar o Auto Farm a qualquer momento, digite:")
warn("   _G.AutoFarm = false")
warn("   StopAutoFarm()")
warn("\nVocê pode controlar as funcionalidades adicionais:")
warn("   _G.AutoQuest = true/false (Ativar/desativar aceitar/entregar quests)")
warn("   _G.AutoAttack = true/false (Ativar/desativar ataque automático)")

-- Descomente a linha abaixo e ajuste _G.AutoQuest e _G.AutoAttack para iniciar automaticamente
-- _G.AutoFarm = true
-- _G.AutoQuest = true
-- _G.AutoAttack = true
-- spawn(StartAutoFarm)
