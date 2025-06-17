_G.AutoFarm = true
_G.Weapon = "Combat"

function TP(pos)
    local hrp = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        hrp.CFrame = pos
        wait(0.1)
    end
end

function EquipWeapon(name)
    local tool = game.Players.LocalPlayer.Backpack:FindFirstChild(name)
    if tool then
        game.Players.LocalPlayer.Character.Humanoid:EquipTool(tool)
        wait(0.2)
    end
end

function Attack()
    local Combat = require(game.Players.LocalPlayer.PlayerScripts.CombatFramework)
    local Controller = Combat.activeController
    if Controller and Controller.equipped then
        Controller:attack()
    end
end

function FindMob(name)
    for _, mob in pairs(workspace.Enemies:GetChildren()) do
        if mob.Name:match(name) and mob:FindFirstChild("Humanoid") and mob.Humanoid.Health > 0 then
            mob:WaitForChild("HumanoidRootPart")
            return mob
        end
    end
end

function CheckQuest()
    local level = game.Players.LocalPlayer.Data.Level.Value
    local placeId = game.PlaceId

    local World1 = (placeId == 2753915549)
    local World2 = (placeId == 4442272183)
    local World3 = (placeId == 7449423635)

    if World1 then
        if level <= 9 then
            NameMon = "Bandit"
            LevelQuest = 1
            NameQuest = "BanditQuest1"
            CFrameQuest = CFrame.new(1059, 16, 1544)
            CFrameMon = CFrame.new(1046, 27, 1561)
        elseif level <= 14 then
            NameMon = "Monkey"
            LevelQuest = 1
            NameQuest = "JungleQuest"
            CFrameQuest = CFrame.new(-1598, 36, 153)
            CFrameMon = CFrame.new(-1449, 68, 11)
        elseif level <= 29 then
            NameMon = "Gorilla"
            LevelQuest = 2
            NameQuest = "JungleQuest"
            CFrameQuest = CFrame.new(-1598, 36, 153)
            CFrameMon = CFrame.new(-1130, 40, -525)
        end
    elseif World2 then
        if level <= 724 then
            NameMon = "Raider"
            LevelQuest = 1
            NameQuest = "Area1Quest"
            CFrameQuest = CFrame.new(-429, 71, 1836)
            CFrameMon = CFrame.new(-728, 52, 2345)
        elseif level <= 774 then
            NameMon = "Mercenary"
            LevelQuest = 2
            NameQuest = "Area1Quest"
            CFrameQuest = CFrame.new(-429, 71, 1836)
            CFrameMon = CFrame.new(-1004, 80, 1424)
        end
    elseif World3 then
        if level <= 1524 then
            NameMon = "Pirate Millionaire"
            LevelQuest = 1
            NameQuest = "PiratePortQuest"
            CFrameQuest = CFrame.new(-290, 42, 5581)
            CFrameMon = CFrame.new(-246, 47, 5584)
        elseif level <= 1574 then
            NameMon = "Pistol Billionaire"
            LevelQuest = 2
            NameQuest = "PiratePortQuest"
            CFrameQuest = CFrame.new(-290, 42, 5581)
            CFrameMon = CFrame.new(-187, 86, 6013)
        end
    end
end

spawn(function()
    while _G.AutoFarm do
        pcall(function()
            CheckQuest()

            if not game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible then
                TP(CFrameQuest)
                wait(1)
                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", NameQuest, LevelQuest)
            else
                local mob = FindMob(NameMon)
                if mob then
                    EquipWeapon(_G.Weapon)
                    TP(mob.HumanoidRootPart.CFrame * CFrame.new(0, 10, 5))
                    wait(0.2)
                    Attack()
                else
                    TP(CFrameMon)
                    wait(1)
                end
            end
        end)
        wait()
    end
end)
