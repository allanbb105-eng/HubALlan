_G.AutoFarm = true
_G.Weapon = "Combat" -- troque pelo nome da sua arma

-- Função para teleportar
function TP(pos)
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = pos
end

-- Checa o nível e define a missão
function GetQuestData()
    local level = game.Players.LocalPlayer.Data.Level.Value
    if level <= 9 then
        return {quest = "BanditQuest1", lvl = 1, mob = "Bandit", mobPos = CFrame.new(1150, 20, 1548), questPos = CFrame.new(1061, 16, 1544)}
    elseif level <= 14 then
        return {quest = "JungleQuest", lvl = 1, mob = "Monkey", mobPos = CFrame.new(-1500, 40, 100), questPos = CFrame.new(-1604, 36, 154)}
    -- Adicione os outros intervalos aqui conforme quiser
    else
        return nil
    end
end

-- Equipar arma
function EquipWeapon(name)
    if game.Players.LocalPlayer.Backpack:FindFirstChild(name) then
        game.Players.LocalPlayer.Character.Humanoid:EquipTool(game.Players.LocalPlayer.Backpack:FindFirstChild(name))
    end
end

-- Pega o inimigo certo
function FindMob(name)
    for _, v in pairs(game.Workspace.Enemies:GetChildren()) do
        if v.Name:match(name) and v.Humanoid.Health > 0 then
            return v
        end
    end
end

-- Loop principal
spawn(function()
    while _G.AutoFarm do
        local q = GetQuestData()
        if q then
            if not game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible then
                TP(q.questPos)
                wait(1)
                game.ReplicatedStorage.Remotes.CommF_:InvokeServer("StartQuest", q.quest, q.lvl)
            else
                local enemy = FindMob(q.mob)
                if enemy then
                    EquipWeapon(_G.Weapon)
                    TP(enemy.HumanoidRootPart.CFrame * CFrame.new(0, 10, 5))
                    require(game.Players.LocalPlayer.PlayerScripts.CombatFramework).activeController:attack()
                else
                    TP(q.mobPos)
                end
            end
        end
        wait()
    end
end)
