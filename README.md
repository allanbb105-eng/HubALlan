-- =====================================================================================================
--                          Bloco 1: Configuração Inicial e Variáveis Globais
-- =====================================================================================================

-- Determina em qual "mar" (mundo) do Blox Fruits o jogador está.
-- O 'PlaceId' é um identificador único para cada lugar (jogo ou mapa) no Roblox.
-- Blox Fruits tem PlaceIds diferentes para o Primeiro, Segundo e Terceiro Mar.
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
    -- Se o jogador não estiver em nenhum dos mares conhecidos, o script avisa e o expulsa do jogo.
    -- Isso evita que o script tente funcionar em locais não suportados.
    game:GetService("Players").LocalPlayer:Kick("Local não suportado. Por favor, entre em um dos mares suportados (Blox Fruits).")
end

-- Variáveis Globais (_G): Estas variáveis são acessíveis de qualquer parte do script.
-- Elas funcionam como "flags" para controlar o comportamento do auto-farm.
_G.Remove_Effect = true  -- Se for 'true', o script tentará remover o efeito de "morte" (tela escura/cinza).
                         -- Isso é útil para visibilidade e para evitar bugs visuais.

_G.AutoFarm = true       -- <--- DEFINIDO COMO TRUE POR PADRÃO
                         -- A principal flag para ligar/desligar o ciclo de auto-farm.
                         -- 'true' = farm ativo, 'false' = farm inativo.

_G.AutoQuest = true      -- <--- DEFINIDO COMO TRUE POR PADRÃO
                         -- Controla se o script deve automaticamente pegar e entregar quests.
                         -- 'true' = tenta pegar quests, 'false' = apenas farma mobs sem quest.

_G.AutoAttack = true     -- <--- DEFINIDO COMO TRUE POR PADRÃO
                         -- Controla se o script deve atacar os monstros.
                         -- 'true' = ataca mobs, 'false' = não ataca (útil para debug ou outros usos).

_G.TargetMob = nil       -- Uma variável para armazenar uma referência ao monstro que está sendo alvo no momento.
                         -- 'nil' quando não há alvo.


-- =====================================================================================================
--                            Bloco 2: Funções Auxiliares Essenciais
-- =====================================================================================================

-- Função: Hop()
-- Propósito: Teletransporta o jogador para um novo servidor do mesmo jogo.
-- Usado para: Evitar administradores, sair de servidores cheios, ou resetar o ambiente se algo bugar.
function Hop()
    print("[Hop] Tentando trocar de servidor...")
    local TeleportService = game:GetService("TeleportService") -- Serviço para teletransporte.
    local Players = game:GetService("Players")                 -- Serviço para acessar jogadores.
    local LocalPlayer = Players.LocalPlayer                   -- O jogador local (você).

    if LocalPlayer then
        -- Teleporta o jogador para o mesmo PlaceId, o que efetivamente o envia para outro servidor.
        TeleportService:Teleport(game.PlaceId, LocalPlayer)
    else
        warn("[Hop] LocalPlayer não encontrado, não é possível teletransportar para outro servidor.")
    end
end

-- Função: Teleport(cframe)
-- Propósito: Move o personagem do jogador instantaneamente para uma coordenada específica (CFrame).
-- 'cframe' é um tipo de dado no Roblox que inclui posição e orientação no espaço 3D.
-- Usado para: Ir até NPCs de quest, locais de spawn de mobs, ou se aproximar de um alvo.
function Teleport(cframe)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    -- Verifica se o jogador e seu HumanoidRootPart (a parte central do personagem) existem.
    if LocalPlayer and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        -- Define a CFrame do HumanoidRootPart do jogador para a CFrame fornecida.
        LocalPlayer.Character.HumanoidRootPart.CFrame = cframe
        print("[Teleport] Teletransportado para: X=" .. math.floor(cframe.Position.X) .. ", Y=" .. math.floor(cframe.Position.Y) .. ", Z=" .. math.floor(cframe.Position.Z))
    else
        warn("[Teleport] HumanoidRootPart do jogador não encontrada para teletransporte. Personagem pode não estar carregado.")
    end
end

-- Função: FindMob(name)
-- Propósito: Encontra o monstro mais próximo do jogador com um nome específico no Workspace (o mundo do jogo).
-- 'name' é o nome do modelo do monstro que o script deve procurar.
-- Usado para: Identificar qual monstro atacar.
function FindMob(name)
    local workspace = game:GetService("Workspace") -- O serviço que contém todos os objetos do mundo.
    local foundMob = nil                           -- Variável para armazenar o monstro encontrado.
    local shortestDistance = math.huge             -- Variável para rastrear a menor distância (inicia com um valor muito grande).
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    if not PlayerHRP then -- Se o HumanoidRootPart do jogador não existir, não podemos calcular distâncias.
        return nil
    end

    -- Percorre todos os objetos dentro do Workspace.
    for i, v in pairs(workspace:GetChildren()) do
        -- Condições para considerar um objeto como um mob alvo:
        if v:IsA("Model") and                   -- É um modelo (geralmente como mobs são agrupados).
           v.Name:find(name, 1, true) and     -- O nome do modelo contém o 'name' procurado (case-sensitive).
           v:FindFirstChild("Humanoid") and     -- Tem um Humanoid (componente que dá vida e movimento).
           v.Humanoid.Health > 0 and          -- Está vivo (saúde maior que 0).
           v:FindFirstChild("HumanoidRootPart") then -- Tem um HumanoidRootPart (para cálculo de distância).

            local mobHRP = v.HumanoidRootPart
            local distance = (PlayerHRP.Position - mobHRP.Position).magnitude -- Calcula a distância entre o jogador e o mob.

            if distance < shortestDistance then -- Se este mob estiver mais perto que o mob mais próximo encontrado até agora...
                shortestDistance = distance    -- Atualiza a menor distância.
                foundMob = v                   -- Este é o novo mob mais próximo.
            end
        end
    end
    return foundMob -- Retorna o monstro mais próximo (ou nil se nenhum for encontrado).
end

-- Função: AttackTarget(target)
-- Propósito: Tenta fazer o jogador atacar um monstro específico.
-- 'target' é o modelo do monstro a ser atacado.
-- ESTA É UMA DAS FUNÇÕES MAIS CRÍTICAS E PROVÁVEL CAUSA DE FALHA.
function AttackTarget(target)
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    -- Validação: Garante que o jogador e o alvo são válidos e estão vivos.
    if not (PlayerHRP and target and target:FindFirstChild("HumanoidRootPart") and target:FindFirstChild("Humanoid") and target.Humanoid.Health > 0) then
        return
    end

    -- Aponta o personagem do jogador para o alvo.
    PlayerHRP.CFrame = CFrame.new(PlayerHRP.Position, target.HumanoidRootPart.Position)

    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local AttackRemote = nil -- Variável para armazenar o RemoteEvent de ataque.

    -- *** TENTATIVAS DE ENCONTRAR O REMOTE EVENT DE ATAQUE ***
    -- A maioria dos jogos Roblox usa RemoteEvents para processar ataques no servidor.
    -- Se o nome do RemoteEvent não for exato, o ataque não registrará.
    -- Você PRECISA usar um Remote Spy no seu executor (ex: Synapse X, Script-Ware) para descobrir
    -- o caminho EXATO e o nome do RemoteEvent que o Blox Fruits usa para ataques M1 (ataque básico).

    -- TENTATIVA 1: Nome comum para RemoteEvent de ataque
    AttackRemote = ReplicatedStorage:FindFirstChild("Remote")
    if AttackRemote and AttackRemote:IsA("RemoteEvent") then
        -- TENTATIVA DE ARGUMENTOS PARA ATAQUE BÁSICO (M1)
        -- O Remote Spy também mostrará os argumentos que o jogo envia.
        -- Blox Fruits geralmente usa o HumanoidRootPart do alvo.
        AttackRemote:FireServer("Attack", target.HumanoidRootPart)
        print("[Attack] Tentando atacar via RemoteEvent 'Remote' com 'Attack' e alvo: " .. target.Name)
        wait(0.2) return -- Se funcionou, sai da função.
    end

    -- TENTATIVA 2: Outra variação comum de RemoteEvent para ataque
    AttackRemote = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Attack")
    if AttackRemote and AttackRemote:IsA("RemoteEvent") then
        -- Argumentos para esta variação
        AttackRemote:FireServer(target.HumanoidRootPart) -- Às vezes, apenas o alvo é o argumento
        print("[Attack] Tentando atacar via RemoteEvent 'Events.Attack' com alvo: " .. target.Name)
        wait(0.2) return
    end

    -- TENTATIVA 3: Variação comum de RemoteEvent para habilidades (pode ser o mesmo para M1)
    AttackRemote = ReplicatedStorage:FindFirstChild("CombatEvents") and ReplicatedStorage.CombatEvents:FindFirstChild("Damage")
    if AttackRemote and AttackRemote:IsA("RemoteEvent") then
        -- Argumentos para esta variação
        AttackRemote:FireServer(target.HumanoidRootPart, "M1") -- Pode precisar de um tipo de ataque
        print("[Attack] Tentando atacar via RemoteEvent 'CombatEvents.Damage' com alvo e tipo: " .. target.Name)
        wait(0.2) return
    end

    -- Adicione mais tentativas aqui se seu Remote Spy revelar outros nomes!
    -- Exemplo: AttackRemote = game.ReplicatedStorage.RemoteNameAqui.SubRemoteAqui
    -- Depois: AttackRemote:FireServer(arg1, arg2, ...)

    -- Fallback: Se nenhum RemoteEvent de ataque for encontrado/funcionar, tenta simular um clique do mouse.
    -- IMPORTANTE: Em jogos como Blox Fruits, a simulação de clique do mouse GERALMENTE NÃO causa dano.
    -- Isso é porque o dano é processado pelo servidor via RemoteEvents.
    warn("[Attack] Nenhum RemoteEvent de ataque funcional encontrado! Tentando simular clique do mouse (provavelmente não dará dano).")
    game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Down:fire()
    wait(0.1) -- Tempo suficiente para o clique ser registrado
    game:GetService("Players").LocalPlayer:GetService("Mouse").Button1Up:fire()
    wait(0.2)
end


-- =====================================================================================================
--                            Bloco 3: Lógica de Quests (CheckQuest)
-- =====================================================================================================

-- Função: CheckQuest()
-- Propósito: Determina qual quest o jogador deve pegar com base no seu nível atual.
-- Retorna: Uma tabela (similar a um dicionário) com todas as informações necessárias para a quest.
-- ATENÇÃO: VOCÊ PRECISA PREENCHER AS COORDENADAS (CFrames) E NOMES DE NPCS/MOBS CORRETOS AQUI!
function CheckQuest()
    local LocalPlayer = game:GetService("Players").LocalPlayer
    local MyLevel = LocalPlayer.Data.Level.Value -- Pega o nível atual do jogador.
    local questData = {} -- Tabela para armazenar os dados da quest.

    -- Lógica para o Primeiro Mar (World1)
    if World1 then
        if MyLevel >= 1 and MyLevel <= 9 then
            questData = {
                Mon = "Bandit",             -- Nome do modelo do monstro no Workspace (EXATAMENTE como aparece no Explorer do Roblox).
                LevelQuest = 1,             -- Nível da quest (para referência).
                NameQuest = "Bandit Quest", -- Nome legível da quest (para logs/GUI).
                NameMon = "Bandit",         -- Nome comum do monstro.
                NPCName = "Bandit Quest Giver", -- Nome do NPC da quest no Workspace.
                CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544), -- CFrame exata do NPC.
                CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125) -- CFrame da área de spawn dos monstros.
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
            -- **IMPORTANTE: VOCÊ PRECISA OBTER AS CFRAMES CORRETAS AQUI!**
            -- Use seu executor para obter as coordenadas do NPC da quest e da área dos mobs.
            questData = {
                Mon = "Jungle Pirate",           -- EX: "Jungle Pirate", "Marine"
                LevelQuest = 20,
                NameQuest = "Jungle Pirate Quest",
                NameMon = "Jungle Pirate",
                NPCName = "Jungle Pirate Quest Giver",      -- EX: "Jungle Pirate Quest Giver"
                CFrameQuest = CFrame.new(0,0,0), -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame PELA DO NPC
                CFrameMon = CFrame.new(0,0,0)    -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame PELA DO LOCAL DE SPAWN DOS MOBS
            }
        -- [[ ADICIONE MAIS BLOCOS 'elseif' PARA OUTRAS QUESTS EM WORLD1 AQUI (copie e cole o padrão) ]]
        end
    -- Lógica para o Segundo Mar (World2)
    elseif World2 then
        if MyLevel >= 700 and MyLevel <= 749 then
            questData = {
                Mon = "Marine Captain", -- Exemplo
                LevelQuest = 700,
                NameQuest = "Marine Captain Quest",
                NameMon = "Marine Captain",
                NPCName = "Marine Captain Quest Giver",
                CFrameQuest = CFrame.new(0,0,0), -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
                CFrameMon = CFrame.new(0,0,0) -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
            }
        end
    -- Lógica para o Terceiro Mar (World3)
    elseif World3 then
        if MyLevel >= 1500 and MyLevel <= 1549 then
            questData = {
                Mon = "Ice Admiral", -- Exemplo
                LevelQuest = 1500,
                NameQuest = "Ice Admiral Quest",
                NameMon = "Ice Admiral",
                NPCName = "Ice Admiral Quest Giver",
                CFrameQuest = CFrame.new(0,0,0), -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
                CFrameMon = CFrame.new(0,0,0) -- <<<<<<<<<<<<<<< SUBSTITUA ESTA CFrame
            }
        -- [[ ADICIONE MAIS BLOCOS 'elseif' PARA OUTRAS QUESTS EM WORLD3 AQUI ]]
        end
    end
    return questData -- Retorna a tabela com os dados da quest.
end

-- Função: InteractWithNPC(npcName)
-- Propósito: Tenta fazer o jogador interagir com um NPC específico (para pegar/entregar quests).
-- 'npcName' é o nome do NPC no Workspace.
-- ESTA É A OUTRA FUNÇÃO MAIS CRÍTICA E PROVÁVEL CAUSA DE FALHA.
function InteractWithNPC(npcName)
    local npc = game:GetService("Workspace"):FindFirstChild(npcName) -- Encontra o NPC pelo nome.
    -- Validação: Garante que o NPC é um modelo válido e tem um HumanoidRootPart.
    if npc and npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") then
        local LocalPlayer = game:GetService("Players").LocalPlayer
        local PlayerHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

        if PlayerHRP then
            -- Teleporta para uma posição ligeiramente à frente do NPC para garantir a interação.
            Teleport(npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 5))
            wait(0.5) -- Espera um pouco para o teletransporte e carregamento.

            local ReplicatedStorage = game:GetService("ReplicatedStorage")
            local InteractRemote = nil -- Variável para armazenar o RemoteEvent de interação.

            -- *** TENTATIVAS DE ENCONTRAR O REMOTE EVENT DE INTERAÇÃO COM NPC ***
            -- Assim como os ataques, as interações com NPCs (pegar/entregar quests) são feitas via RemoteEvents.
            -- Você PRECISA usar um Remote Spy para ver EXATAMENTE qual RemoteEvent e quais argumentos
            -- são disparados quando você aceita ou entrega uma quest manualmente.

            -- TENTATIVA 1: Nome comum para RemoteEvent
            InteractRemote = ReplicatedStorage:FindFirstChild("Remote")
            if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                -- TENTATIVA DE ARGUMENTOS PARA INTERAÇÃO
                -- Blox Fruits é conhecido por usar "Quest" como primeiro argumento para quests.
                InteractRemote:FireServer("Quest", npc.Name) -- Variação 1
                print("[NPC Interaction] Tentando interagir com NPC '" .. npc.Name .. "' via 'Remote' com 'Quest'.")
                wait(0.5) return
            end

            -- TENTATIVA 2: Simular clique no NPC
            InteractRemote = ReplicatedStorage:FindFirstChild("Remote") -- Reutiliza o mesmo remote, mas com argumentos de clique
            if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                -- Esta é uma variação muito comum em Blox Fruits para interações gerais de clique.
                InteractRemote:FireServer("Evt", npc.Name, "Click") -- "Evt" para evento, nome do NPC, e "Click"
                print("[NPC Interaction] Tentando interagir com NPC '" .. npc.Name .. "' via 'Remote' com 'Evt', 'Click'.")
                wait(0.5) return
            end
            
            -- TENTATIVA 3: Outra variação para eventos/interações
            InteractRemote = ReplicatedStorage:FindFirstChild("Events") and ReplicatedStorage.Events:FindFirstChild("Interact")
            if InteractRemote and InteractRemote:IsA("RemoteEvent") then
                InteractRemote:FireServer(npc.Name, "Quest") -- Pode ser o nome do NPC primeiro
                print("[NPC Interaction] Tentando interagir com NPC '" .. npc.Name .. "' via 'Events.Interact'.")
                wait(0.5) return
            end

            -- Adicione mais tentativas aqui se seu Remote Spy revelar outros nomes!
            -- Exemplo: InteractRemote = game.ReplicatedStorage.OutroRemoteAqui
            -- Depois: InteractRemote:FireServer(arg1, arg2, ...)

            warn("[NPC Interaction] Nenhum RemoteEvent de interação funcional encontrado para '" .. npc.Name .. "'! A interação com o NPC pode falhar.")
        end
    else
        warn("[NPC Interaction] NPC '" .. npcName .. "' não encontrado para interação. Verifique o nome do NPC e a CFrame em CheckQuest().")
    end
end

-- =====================================================================================================
--                            Bloco 4: Função Principal de Auto-Farm
-- =====================================================================================================

local autoFarmThread = nil -- Variável para manter a referência à thread de farm, para poder pará-la.

-- Função: StartAutoFarm()
-- Propósito: Inicia o ciclo principal de auto-farm.
function StartAutoFarm()
    -- Evita iniciar múltiplas instâncias do auto-farm se já estiver ativo.
    if _G.AutoFarm and autoFarmThread then
        print("[AutoFarm] Auto Farm já está em execução.")
        return
    end

    _G.AutoFarm = true -- Define a flag global para ativo.
    print("====================================")
    print("           Auto Farm INICIADO!      ")
    print("====================================")
    updateStatusText("Status: Ativo") -- Atualiza o texto na GUI

    -- 'spawn(function() ... end)' cria uma nova thread de execução, permitindo que o script
    -- continue rodando sem travar o jogo.
    autoFarmThread = spawn(function()
        while _G.AutoFarm do -- O loop principal que continua enquanto o auto-farm estiver ativo.
            local questData = CheckQuest() -- Obtém as informações da quest para o nível atual.

            -- Verifica se as informações essenciais da quest foram obtidas.
            -- Se não houver dados válidos, o script avisará mas continuará o loop,
            -- esperando que os dados sejam corrigidos ou que o nível mude.
            if not questData.Mon then warn("Quest data missing 'Mon'. Check CheckQuest().") end
            if not questData.NPCName then warn("Quest data missing 'NPCName'. Check CheckQuest().") end
            if not questData.CFrameQuest then warn("Quest data missing 'CFrameQuest'. Check CheckQuest().") end
            if not questData.CFrameMon then warn("Quest data missing 'CFrameMon'. Check CheckQuest().") end

            local questNPCName = questData.NPCName
            local mobName = questData.Mon
            local questCFrame = questData.CFrameQuest
            local mobSpawnCFrame = questData.CFrameMon
            local mobsToKill = 5 -- Quantidade de mobs para matar por quest (ajuste conforme o jogo).

            print("\n--- Ciclo de Farm para Nível " .. game:GetService("Players").LocalPlayer.Data.Level.Value .. " (Quest: " .. (questData.NameQuest or "Desconhecida") .. ") ---")

            -- 1. Aceitar/Completar Quest (se _G.AutoQuest estiver ativado)
            if _G.AutoQuest and questNPCName and questCFrame then
                print("[AutoQuest] Teletransportando para o NPC da Quest: " .. questNPCName)
                Teleport(questCFrame)
                wait(1.5) -- Espera para o mapa carregar ou o personagem se estabilizar.
                InteractWithNPC(questNPCName) -- Tenta interagir para aceitar/entregar.
                wait(1.5) -- Espera após a interação.
            elseif _G.AutoQuest then
                warn("[AutoQuest] Não foi possível interagir com o NPC da quest. Verifique NPCName e CFrameQuest em CheckQuest().")
            end

            -- 2. Teletransportar para o local de spawn dos monstros
            if mobName and mobSpawnCFrame then
                print("[Farm] Teletransportando para a área de farm de: " .. mobName)
                Teleport(mobSpawnCFrame)
                wait(1.5) -- Espera para o carregamento do mapa e spawn dos mobs.

                local mobsKilled = 0       -- Contador de mobs abatidos.
                local attemptCount = 0     -- Contador de tentativas de encontrar mob (para evitar loops infinitos).

                -- Loop para encontrar e atacar os monstros até o número desejado de abates.
                while mobsKilled < mobsToKill and _G.AutoFarm and attemptCount < (mobsToKill * 20) do
                    _G.TargetMob = FindMob(mobName) -- Tenta encontrar o monstro mais próximo.

                    if _G.TargetMob then
                        print("[Farm] Atacando: " .. _G.TargetMob.Name .. " (" .. mobsKilled .. "/" .. mobsToKill .. " abatidos)")
                        if _G.AutoAttack then
                            AttackTarget(_G.TargetMob) -- Chama a função para atacar o monstro.
                        end
                        
                        local attackAttempts = 0 -- Contador de tentativas de ataque para o monstro atual.
                        local LocalPlayer = game:GetService("Players").LocalPlayer
                        -- Loop para continuar atacando o mesmo monstro até ele morrer ou um limite de tentativas.
                        while _G.TargetMob and _G.TargetMob:FindFirstChild("Humanoid") and _G.TargetMob.Humanoid.Health > 0 and attackAttempts < 50 and _G.AutoFarm do
                            if _G.AutoAttack then
                                AttackTarget(_G.TargetMob)
                            end
                            wait(0.2) -- Pequeno delay entre ataques.
                            attackAttempts = attackAttempts + 1
                            -- Se o jogador se afastar muito do alvo, teletransporta-se de volta.
                            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and _G.TargetMob:FindFirstChild("HumanoidRootPart") then
                                if (LocalPlayer.Character.HumanoidRootPart.Position - _G.TargetMob.HumanoidRootPart.Position).magnitude > 20 then
                                    Teleport(_G.TargetMob.HumanoidRootPart.CFrame * CFrame.new(0,0,5))
                                    wait(0.5)
                                end
                            end
                        end

                        -- Verifica se o monstro foi derrotado.
                        if not _G.TargetMob or not _G.TargetMob:FindFirstChild("Humanoid") or _G.TargetMob.Humanoid.Health <= 0 then
                            print("[Farm] Monstro derrotado: " .. mobName)
                            mobsKilled = mobsKilled + 1
                            _G.TargetMob = nil -- Limpa o alvo para buscar o próximo.
                            wait(0.5) -- Pequeno delay antes de procurar o próximo monstro.
                        else
                            warn("[Farm] Monstro '" .. mobName .. "' não derrotado ou problema no alvo. Buscando próximo. Saúde restante: " .. _G.TargetMob.Humanoid.Health)
                            _G.TargetMob = nil -- Se não derrotou, tenta o próximo no loop.
                        end
                    else
                        print("[Farm] Nenhum monstro '" .. mobName .. "' encontrado. Esperando o spawn ou procurando novamente...")
                        wait(2) -- Espera antes de tentar encontrar novamente.
                    end
                    attemptCount = attemptCount + 1
                    wait(0.1) -- Pequeno delay para evitar sobrecarga.
                end

                if mobsKilled >= mobsToKill then
                    print("[Farm] Mobs suficientes derrotados (" .. mobsKilled .. "/" .. mobsToKill .. ") para a quest de " .. mobName .. ".")
                else
                    warn("[Farm] Não foi possível derrotar todos os mobs para a quest atual. Abatidos: " .. mobsKilled .. ". Verifique se as CFrames e os nomes dos mobs estão corretos ou se os remotes de ataque estão funcionando.")
                end
            else
                warn("[Farm] Não foi possível iniciar o farm de mobs. Verifique o nome do monstro e a CFrame de spawn em CheckQuest().")
            end

            -- 3. Voltar para o NPC da Quest para entregar (se _G.AutoQuest estiver ativado)
            if _G.AutoQuest and questNPCName and questCFrame then
                print("[AutoQuest] Voltando para o NPC da Quest para entregar: " .. questNPCName)
                Teleport(questCFrame)
                wait(1.5)
                InteractWithNPC(questNPCName)
                wait(1.5)
            elseif _G.AutoQuest then
                warn("[AutoQuest] Não foi possível entregar a quest. Verifique NPCName e CFrameQuest em CheckQuest().")
            end

            wait(2) -- Pequeno delay antes de iniciar o próximo ciclo de farm.
        end
        print("====================================")
        print("          Auto Farm PARADO.         ")
        print("====================================")
        updateStatusText("Status: Inativo") -- Atualiza o texto na GUI
        autoFarmThread = nil -- Reseta a thread.
    end)
end

-- Função: StopAutoFarm()
-- Propósito: Para o ciclo de auto-farm.
function StopAutoFarm()
    _G.AutoFarm = false -- Define a flag global para inativo, o que fará o loop 'while _G.AutoFarm' terminar.
    _G.TargetMob = nil  -- Limpa o alvo atual.
    print("[Controle] Sinal para parar Auto Farm enviado.")
    updateStatusText("Status: Inativo") -- Atualiza o texto na GUI
end


-- =====================================================================================================
--                            Bloco 5: Threads e Listeners Secundários
-- =====================================================================================================

-- Thread para remover efeitos visuais (como o efeito de morte na tela).
spawn(function()
    game:GetService('RunService').Stepped:Connect(function() -- 'Stepped' é um evento que dispara a cada frame.
        if _G.Remove_Effect then
            -- Procura e destrói quaisquer efeitos de "Death" (morte) na tela.
            for i, v in pairs(game:GetService("ReplicatedStorage").Effect.Container:GetChildren()) do
                if v.Name == "Death" then
                    v:Destroy()
                end
            end
        end
    end)
end)

-- Thread para detectar administradores ou jogadores específicos e dar "Hop" (trocar de servidor).
spawn(function()
    while wait(5) do -- Verifica a cada 5 segundos.
        for i,v in pairs(game.Players:GetPlayers()) do -- Percorre todos os jogadores no servidor.
            -- Lista de nomes de administradores ou jogadores a serem evitados.
            if v.Name == "red_game43" or v.Name == "rip_indra" or v.Name == "Axiore" or v.Name == "Polkster" or v.Name == "wenlocktoad" or v.Name == "Daigrock" or v.Name == "toilamvidamme" or v.Name == "oofficialnoobie" or v.Name == "Uzoth" or v.Name == "Azarth" or v.Name == "arlthmetic" or v.Name == "Death_King" or v.Name == "Lunoven" or v.Name == "TheGreateAced" or v.Name == "rip_fud" or v.Name == "drip_mama" or v.Name == "layandikit12" or v.Name == "Hingoi" then
                warn("[Anti-Admin] Admin/Player específico detectado: " .. v.Name .. ". Dando Hop!")
                Hop() -- Chama a função para trocar de servidor.
                wait(20) -- Espera um pouco após o hop para não loopar excessivamente em servidores com admins.
                break -- Sai do loop de jogadores após detectar um admin e dar hop.
            end
        end
    end
end)

-- Anti-AFK: Previni que o jogador seja chutado por inatividade.
spawn(function()
    while wait(15) do -- A cada 15 segundos.
        if _G.AutoFarm then -- Só funciona se o auto-farm estiver ativo.
            local LocalPlayer = game:GetService("Players").LocalPlayer
            local Humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
            if Humanoid then
                -- Alterna o estado do Humanoid para simular movimento e depois volta para ocioso.
                -- Isso engana o sistema AFK do jogo.
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
local coreGui = game:GetService("CoreGui") -- Geralmente o local para criar GUIs em exploits

-- Cria a ScreenGui (o contêiner principal para todos os elementos da GUI na tela)
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AutoFarmGUI"
ScreenGui.Parent = coreGui -- Em exploits, é comum parentar para CoreGui. Alternativa: player.PlayerGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling -- Garante que a GUI fique por cima de outras GUIs do jogo

-- Cria o Frame principal (a janela retangular que você vê)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 220, 0, 180) -- Largura de 220 pixels, altura de 180 pixels
MainFrame.Position = UDim2.new(0.5, -110, 0.5, -90) -- Centraliza o frame na tela (0.5 = 50% da tela, -110 = metade da largura para centralizar)
MainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40) -- Cor de fundo escura
MainFrame.BorderSizePixel = 0 -- Sem borda padrão
MainFrame.Draggable = true -- Permite arrastar o frame com o mouse
MainFrame.Parent = ScreenGui

-- Título da GUI
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "TitleLabel"
TitleLabel.Size = UDim2.new(1, 0, 0, 30) -- Ocupa 100% da largura do pai, 30 pixels de altura
TitleLabel.Position = UDim2.new(0, 0, 0, 0) -- No topo do frame
TitleLabel.BackgroundColor3 = Color3.fromRGB(60, 60, 60) -- Cor de fundo para o título
TitleLabel.Text = "⚡ Auto Farm Blox Fruits ⚡"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255) -- Texto branco
TitleLabel.Font = Enum.Font.SourceSansBold -- Fonte negrito
TitleLabel.TextSize = 18
TitleLabel.BorderSizePixel = 0
TitleLabel.Parent = MainFrame

-- Label para exibir o status do Auto Farm
local StatusLabel = Instance.new("TextLabel")
StatusLabel.Name = "StatusLabel"
StatusLabel.Size = UDim2.new(1, 0, 0, 20)
StatusLabel.Position = UDim2.new(0, 0, 0, 30) -- Abaixo do título
StatusLabel.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
StatusLabel.Text = "Status: Inativo" -- Texto inicial
StatusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
StatusLabel.Font = Enum.Font.SourceSans
StatusLabel.TextSize = 14
StatusLabel.BorderSizePixel = 0
StatusLabel.Parent = MainFrame

-- Função auxiliar para atualizar o texto do StatusLabel
local function updateStatusText(text)
    StatusLabel.Text = text
end

-- Função auxiliar para atualizar o texto e a cor dos botões de toggle (ON/OFF)
local function updateToggleButton(button, value)
    if value then
        button.Text = button.Name .. ": ON"
        button.BackgroundColor3 = Color3.fromRGB(70, 150, 70) -- Verde para ON
    else
        button.Text = button.Name .. ": OFF"
        button.BackgroundColor3 = Color3.fromRGB(150, 70, 70) -- Vermelho para OFF
    end
end

-- Botão Toggle Auto Farm
local AutoFarmButton = Instance.new("TextButton")
AutoFarmButton.Name = "AutoFarm"
AutoFarmButton.Size = UDim2.new(0.9, 0, 0, 30) -- 90% da largura do pai, 30 de altura
AutoFarmButton.Position = UDim2.new(0.05, 0, 0, 60) -- 5% de padding, 60 pixels abaixo do topo
AutoFarmButton.Font = Enum.Font.SourceSansBold
AutoFarmButton.TextSize = 16
AutoFarmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoFarmButton.BorderSizePixel = 0
AutoFarmButton.Parent = MainFrame
updateToggleButton(AutoFarmButton, _G.AutoFarm) -- Define o estado inicial do botão

AutoFarmButton.MouseButton1Click:Connect(function()
    _G.AutoFarm = not _G.AutoFarm -- Inverte o estado de _G.AutoFarm
    updateToggleButton(AutoFarmButton, _G.AutoFarm) -- Atualiza o visual do botão
    if _G.AutoFarm then
        StartAutoFarm() -- Chama a função para iniciar o farm
    else
        StopAutoFarm() -- Chama a função para parar o farm
    end
end)

-- Botão Toggle Auto Quest
local AutoQuestButton = Instance.new("TextButton")
AutoQuestButton.Name = "AutoQuest"
AutoQuestButton.Size = UDim2.new(0.9, 0, 0, 30)
AutoQuestButton.Position = UDim2.new(0.05, 0, 0, 95) -- 35 pixels abaixo do botão anterior
AutoQuestButton.Font = Enum.Font.SourceSansBold
AutoQuestButton.TextSize = 16
AutoQuestButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoQuestButton.BorderSizePixel = 0
AutoQuestButton.Parent = MainFrame
updateToggleButton(AutoQuestButton, _G.AutoQuest)

AutoQuestButton.MouseButton1Click:Connect(function()
    _G.AutoQuest = not _G.AutoQuest -- Inverte o estado de _G.AutoQuest
    updateToggleButton(AutoQuestButton, _G.AutoQuest) -- Atualiza o visual do botão
    print("[Controle] AutoQuest agora está: " .. tostring(_G.AutoQuest))
end)

-- Botão Toggle Auto Attack
local AutoAttackButton = Instance.new("TextButton")
AutoAttackButton.Name = "AutoAttack"
AutoAttackButton.Size = UDim2.new(0.9, 0, 0, 30)
AutoAttackButton.Position = UDim2.new(0.05, 0, 0, 130) -- 35 pixels abaixo do botão anterior
AutoAttackButton.Font = Enum.Font.SourceSansBold
AutoAttackButton.TextSize = 16
AutoAttackButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoAttackButton.BorderSizePixel = 0
AutoAttackButton.Parent = MainFrame
updateToggleButton(AutoAttackButton, _G.AutoAttack)

AutoAttackButton.MouseButton1Click:Connect(function()
    _G.AutoAttack = not _G.AutoAttack -- Inverte o estado de _G.AutoAttack
    updateToggleButton(AutoAttackButton, _G.AutoAttack) -- Atualiza o visual do botão
    print("[Controle] AutoAttack agora está: " .. tostring(_G.AutoAttack))
end)

-- Botão para Esconder/Mostrar a GUI
local HideShowButton = Instance.new("TextButton")
HideShowButton.Name = "Hide/Show"
HideShowButton.Size = UDim2.new(0.4, 0, 0, 15) -- Metade da largura do frame, menor altura
HideShowButton.Position = UDim2.new(0.05, 0, 0, 165) -- Posição inferior esquerda
HideShowButton.Font = Enum.Font.SourceSans
HideShowButton.TextSize = 12
HideShowButton.TextColor3 = Color3.fromRGB(255, 255, 255)
HideShowButton.BackgroundColor3 = Color3.fromRGB(90, 90, 90)
HideShowButton.Text = "Hide"
HideShowButton.BorderSizePixel = 0
HideShowButton.Parent = MainFrame

HideShowButton.MouseButton1Click:Connect(function()
    if MainFrame.Visible then
        MainFrame.Visible = false -- Esconde o frame
        HideShowButton.Text = "Show"
    else
        MainFrame.Visible = true -- Mostra o frame
        HideShowButton.Text = "Hide"
    end
end)

-- Botão para Sair (Destruir a GUI e parar o script)
local ExitButton = Instance.new("TextButton")
ExitButton.Name = "Exit"
ExitButton.Size = UDim2.new(0.4, 0, 0, 15)
ExitButton.Position = UDim2.new(0.55, 0, 0, 165) -- Posição inferior direita
ExitButton.Font = Enum.Font.SourceSans
ExitButton.TextSize = 12
ExitButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ExitButton.BackgroundColor3 = Color3.fromRGB(180, 50, 50) -- Vermelho para sair
ExitButton.Text = "Exit"
ExitButton.BorderSizePixel = 0
ExitButton.Parent = MainFrame

ExitButton.MouseButton1Click:Connect(function()
    StopAutoFarm() -- Garante que o auto-farm pare antes de sair
    ScreenGui:Destroy() -- Remove a GUI da tela
    warn("Script de Auto Farm finalizado e GUI destruída.")
end)

-- Inicializa o status na GUI quando o script é carregado
-- e já inicia o auto-farm se _G.AutoFarm for true por padrão.
updateStatusText("Status: Ativo (Iniciando...)")
if _G.AutoFarm then
    StartAutoFarm()
end
