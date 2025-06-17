-- ALLAN HUB CORRIGIDO • PARTE 1
local ScreenGui = Instance.new("ScreenGui")
local Toggle = Instance.new("TextButton")
local ModeToggle = Instance.new("TextButton")

ScreenGui.Name = "AllanHub"
ScreenGui.Parent = game:GetService("CoreGui")

Toggle.Name = "AutoFarmToggle"
Toggle.Parent = ScreenGui
Toggle.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Toggle.Position = UDim2.new(0, 10, 0, 100)
Toggle.Size = UDim2.new(0, 150, 0, 40)
Toggle.Font = Enum.Font.SourceSansBold
Toggle.Text = "Ativar Allan Hub"
Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
Toggle.TextSize = 20

ModeToggle.Name = "ModeToggle"
ModeToggle.Parent = ScreenGui
ModeToggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ModeToggle.Position = UDim2.new(0, 10, 0, 150)
ModeToggle.Size = UDim2.new(0, 150, 0, 40)
ModeToggle.Font = Enum.Font.SourceSansBold
ModeToggle.Text = "Modo: BodyPosition"
ModeToggle.TextColor3 = Color3.fromRGB(255, 255, 255)
ModeToggle.TextSize = 18

_G.AutoFarm = false
_G.UseTeleport = false
_G.Weapon = "Combat"
_G.AutoFruit = true -- ou usar botão no hub

Toggle.MouseButton1Click:Connect(function()
    _G.AutoFarm = not _G.AutoFarm
    Toggle.Text = _G.AutoFarm and "Desativar Allan Hub" or "Ativar Allan Hub"
end)

ModeToggle.MouseButton1Click:Connect(function()
    _G.UseTeleport = not _G.UseTeleport
    ModeToggle.Text = _G.UseTeleport and "Modo: Teleporte" or "Modo: BodyPosition"
end)

function TP(pos)
    local hrp = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if hrp then hrp.CFrame = pos wait(0.1) end
end

function EquipWeapon(name)
    local tool = game.Players.LocalPlayer.Backpack:FindFirstChild(name)
    if tool then
        game.Players.LocalPlayer.Character.Humanoid:EquipTool(tool)
        wait(0.2)
    end
end

function SimulateClick()
    local vim = game:GetService("VirtualInputManager")
    vim:SendMouseButtonEvent(0, 0, 0, true, game, 0)
    vim:SendMouseButtonEvent(0, 0, 0, false, game, 0)
end

function Attack()
    local success, err = pcall(function()
        local Combat = require(game.Players.LocalPlayer.PlayerScripts.CombatFramework)
        local controller = Combat.activeController
        if controller and controller.equipped then
            for i = 1, 3 do
                controller:attack()
                wait(0.05)
            end
        else
            SimulateClick()
        end
    end)
    if not success then SimulateClick() end
end

function MagnetMobs(center)
    for _, mob in pairs(workspace.Enemies:GetChildren()) do
        if mob:FindFirstChild("HumanoidRootPart") and mob:FindFirstChild("Humanoid") and mob.Humanoid.Health > 0 then
            pcall(function()
                mob.HumanoidRootPart.CFrame = CFrame.new(center + Vector3.new(math.random(-3, 3), 0, math.random(-3, 3)))
            end)
        end
    end
end

function StickToMob(target)
    local hrp = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not (hrp and target) then return end
    local bp = hrp:FindFirstChild("AllanStick") or Instance.new("BodyPosition")
    bp.Name = "AllanStick"
    bp.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bp.Position = target.Position + Vector3.new(0, 0.8, 0)
    bp.P = 5000
    bp.D = 500
    bp.Parent = hrp
end

function ClearStick()
    local hrp = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        local stick = hrp:FindFirstChild("AllanStick")
        if stick then stick:Destroy() end
    end
end
function CheckQuest()
    local MyLevel = game.Players.LocalPlayer.Data.Level.Value
    if game.PlaceId == 2753915549 then World1 = true end
    if game.PlaceId == 4442272183 then World2 = true end
    if game.PlaceId == 7449423635 then World3 = true end
    if World1 then
        if MyLevel <= 9 then
            NameMon = "Bandit"
            LevelQuest = 1
            NameQuest = "BanditQuest1"
            CFrameQuest = CFrame.new(1059.37, 15.44, 1550.42)
            CFrameMon = CFrame.new(1045.96, 27.00, 1560.82)
        elseif MyLevel <= 14 then
            NameMon = "Monkey"
            LevelQuest = 1
            NameQuest = "JungleQuest"
            CFrameQuest = CFrame.new(-1598.08, 35.55, 153.37)
            CFrameMon = CFrame.new(-1448.51, 67.85, 11.46)
        elseif MyLevel <= 29 then
            NameMon = "Gorilla"
            LevelQuest = 2
            NameQuest = "JungleQuest"
            CFrameQuest = CFrame.new(-1598.08, 35.55, 153.37)
            CFrameMon = CFrame.new(-1129.88, 40.46, -525.42)
        elseif MyLevel <= 39 then
            NameMon = "Pirate"
            LevelQuest = 1
            NameQuest = "BuggyQuest1"
            CFrameQuest = CFrame.new(-1141.07, 4.10, 3831.54)
            CFrameMon = CFrame.new(-1103.51, 13.75, 3896.09)
        elseif MyLevel <= 59 then
            NameMon = "Brute"
            LevelQuest = 2
            NameQuest = "BuggyQuest1"
            CFrameQuest = CFrame.new(-1141.07, 4.10, 3831.54)
            CFrameMon = CFrame.new(-1140.08, 14.80, 4322.92)
        elseif MyLevel <= 74 then
            NameMon = "Desert Bandit"
            LevelQuest = 1
            NameQuest = "DesertQuest"
            CFrameQuest = CFrame.new(894.48, 5.14, 4392.43)
            CFrameMon = CFrame.new(924.79, 6.44, 4481.58)
        elseif MyLevel <= 89 then
            NameMon = "Desert Officer"
            LevelQuest = 2
            NameQuest = "DesertQuest"
            CFrameQuest = CFrame.new(894.48, 5.14, 4392.43)
            CFrameMon = CFrame.new(1608.28, 8.61, 4371.00)
        elseif MyLevel <= 99 then
            NameMon = "Snow Bandit"
            LevelQuest = 1
            NameQuest = "SnowQuest"
            CFrameQuest = CFrame.new(1389.74, 88.15, -1298.90)
            CFrameMon = CFrame.new(1354.34, 87.27, -1393.94)
        elseif MyLevel <= 119 then
            NameMon = "Snowman"
            LevelQuest = 2
            NameQuest = "SnowQuest"
            CFrameQuest = CFrame.new(1389.74, 88.15, -1298.90)
            CFrameMon = CFrame.new(1201.64, 144.57, -1550.06)
        -- continua com os próximos níveis...
 elseif MyLevel <= 149 then
            NameMon = "Chief Petty Officer"
            LevelQuest = 1
            NameQuest = "MarineQuest2"
            CFrameQuest = CFrame.new(-5039.58, 27.35, 4324.68)
            CFrameMon = CFrame.new(-4881.23, 22.65, 4273.75)
        elseif MyLevel <= 174 then
            NameMon = "Sky Bandit"
            LevelQuest = 1
            NameQuest = "SkyQuest"
            CFrameQuest = CFrame.new(-4839.53, 716.36, -2619.44)
            CFrameMon = CFrame.new(-4953.20, 295.74, -2899.22)
        elseif MyLevel <= 189 then
            NameMon = "Dark Master"
            LevelQuest = 2
            NameQuest = "SkyQuest"
            CFrameQuest = CFrame.new(-4839.53, 716.36, -2619.44)
            CFrameMon = CFrame.new(-5259.84, 391.39, -2229.03)
        elseif MyLevel <= 209 then
            NameMon = "Prisoner"
            LevelQuest = 1
            NameQuest = "PrisonerQuest"
            CFrameQuest = CFrame.new(5308.93, 1.65, 475.12)
            CFrameMon = CFrame.new(5098.97, -0.32, 474.23)
        elseif MyLevel <= 249 then
            NameMon = "Dangerous Prisoner"
            LevelQuest = 2
            NameQuest = "PrisonerQuest"
            CFrameQuest = CFrame.new(5308.93, 1.65, 475.12)
            CFrameMon = CFrame.new(5654.56, 15.63, 866.29)
        elseif MyLevel <= 274 then
            NameMon = "Toga Warrior"
            LevelQuest = 1
            NameQuest = "ColosseumQuest"
            CFrameQuest = CFrame.new(-1580.04, 6.35, -2986.47)
            CFrameMon = CFrame.new(-1820.21, 51.68, -2740.66)
        elseif MyLevel <= 299 then
            NameMon = "Gladiator"
            LevelQuest = 2
            NameQuest = "ColosseumQuest"
            CFrameQuest = CFrame.new(-1580.04, 6.35, -2986.47)
            CFrameMon = CFrame.new(-1292.83, 56.38, -3339.03)
        elseif MyLevel <= 324 then
            NameMon = "Military Soldier"
            LevelQuest = 1
            NameQuest = "MagmaQuest"
            CFrameQuest = CFrame.new(-5313.37, 10.95, 8515.29)
            CFrameMon = CFrame.new(-5411.16, 11.08, 8454.29)
        elseif MyLevel <= 374 then
            NameMon = "Military Spy"
            LevelQuest = 2
            NameQuest = "MagmaQuest"
            CFrameQuest = CFrame.new(-5313.37, 10.95, 8515.29)
            CFrameMon = CFrame.new(-5802.86, 86.26, 8828.85)
        elseif MyLevel <= 399 then
            NameMon = "Fishman Warrior"
            LevelQuest = 1
            NameQuest = "FishmanQuest"
            CFrameQuest = CFrame.new(61122.65, 18.49, 1569.39)
            CFrameMon = CFrame.new(60878.30, 18.48, 1543.75)
        elseif MyLevel <= 449 then
            NameMon = "Fishman Commando"
            LevelQuest = 2
            NameQuest = "FishmanQuest"
            CFrameQuest = CFrame.new(61122.65, 18.49, 1569.39)
            CFrameMon = CFrame.new(61922.63, 18.48, 1493.93)
        elseif MyLevel <= 474 then
            NameMon = "God's Guard"
            LevelQuest = 1
            NameQuest = "SkyExp1Quest"
            CFrameQuest = CFrame.new(-4721.88, 843.87, -1949.96)
            CFrameMon = CFrame.new(-4710.04, 845.27, -1927.30)
        elseif MyLevel <= 524 then
            NameMon = "Shanda"
            LevelQuest = 2
            NameQuest = "SkyExp1Quest"
            CFrameQuest = CFrame.new(-4721.88, 843.87, -1949.96)
            CFrameMon = CFrame.new(-7678.48, 5566.40, -497.21)
        elseif MyLevel <= 549 then
            NameMon = "Royal Squad"
            LevelQuest = 1
            NameQuest = "SkyExp2Quest"
            CFrameQuest = CFrame.new(-7906.81, 5634.66, -1411.99)
            CFrameMon = CFrame.new(-7624.25, 5658.13, -1467.35)
        elseif MyLevel <= 624 then
            NameMon = "Royal Soldier"
            LevelQuest = 2
            NameQuest = "SkyExp2Quest"
            CFrameQuest = CFrame.new(-7906.81, 5634.66, -1411.99)
            CFrameMon = CFrame.new(-7836.75, 5645.66, -1790.62)
        elseif MyLevel <= 649 then
            NameMon = "Galley Pirate"
            LevelQuest = 1
            NameQuest = "FountainQuest"
            CFrameQuest = CFrame.new(5259.81, 37.35, 4050.02)
            CFrameMon = CFrame.new(5551.02, 78.90, 3930.41)
        elseif MyLevel >= 650 then
            NameMon = "Galley Captain"
            LevelQuest = 2
            NameQuest = "FountainQuest"
            CFrameQuest = CFrame.new(5259.81, 37.35, 4050.02)
            CFrameMon = CFrame.new(5441.95, 42.50, 4950.09)
        end
    end
elseif World2 then
        if MyLevel <= 724 then
            NameMon = "Raider"
            LevelQuest = 1
            NameQuest = "Area1Quest"
            CFrameQuest = CFrame.new(-429, 71, 1836)
            CFrameMon = CFrame.new(-728, 52, 2345)
        elseif MyLevel <= 774 then
            NameMon = "Swan Pirate"
            LevelQuest = 2
            NameQuest = "Area1Quest"
            CFrameQuest = CFrame.new(-429, 71, 1836)
            CFrameMon = CFrame.new(878, 121, 1235)
        elseif MyLevel <= 799 then
            NameMon = "Marine Lieutenant"
            LevelQuest = 1
            NameQuest = "Area2Quest"
            CFrameQuest = CFrame.new(-5035, 29, 851)
            CFrameMon = CFrame.new(-5411, 16, 846)
        elseif MyLevel <= 874 then
            NameMon = "Marine Captain"
            LevelQuest = 2
            NameQuest = "Area2Quest"
            CFrameQuest = CFrame.new(-5035, 29, 851)
            CFrameMon = CFrame.new(-4855, 23, 832)
        elseif MyLevel <= 899 then
            NameMon = "Zombie"
            LevelQuest = 1
            NameQuest = "ZombieQuest"
            CFrameQuest = CFrame.new(-5494, 49, -795)
            CFrameMon = CFrame.new(-5571, 49, -882)
elseif MyLevel <= 949 then
            NameMon = "Vampire"
            LevelQuest = 2
            NameQuest = "ZombieQuest"
            CFrameQuest = CFrame.new(-5494, 49, -795)
            CFrameMon = CFrame.new(-5806, 16, -860)
        elseif MyLevel <= 974 then
            NameMon = "Snow Trooper"
            LevelQuest = 1
            NameQuest = "SnowMountainQuest"
            CFrameQuest = CFrame.new(605, 402, -5312)
            CFrameMon = CFrame.new(590, 402, -5478)
        elseif MyLevel <= 999 then
            NameMon = "Arctic Warrior"
            LevelQuest = 2
            NameQuest = "SnowMountainQuest"
            CFrameQuest = CFrame.new(605, 402, -5312)
            CFrameMon = CFrame.new(610, 402, -5540)
        elseif MyLevel <= 1024 then
            NameMon = "Fishman Warrior"
            LevelQuest = 1
            NameQuest = "IceSideQuest"
            CFrameQuest = CFrame.new(-6072, 15, -4966)
            CFrameMon = CFrame.new(-6113, 15, -5185)
        elseif MyLevel <= 1049 then
            NameMon = "Fishman Commando"
            LevelQuest = 2
            NameQuest = "IceSideQuest"
            CFrameQuest = CFrame.new(-6072, 15, -4966)
            CFrameMon = CFrame.new(-6226, 15, -4978)
        elseif MyLevel <= 1099 then
            NameMon = "Snow Lurker"
            LevelQuest = 1
            NameQuest = "CursedShipQuest"
            CFrameQuest = CFrame.new(916, 123, 32865)
            CFrameMon = CFrame.new(967, 123, 32909)
        elseif MyLevel <= 1124 then
            NameMon = "Ship Deckhand"
            LevelQuest = 1
            NameQuest = "ShipQuest1"
            CFrameQuest = CFrame.new(1038, 125, 32864)
            CFrameMon = CFrame.new(1145, 125, 32888)
        elseif MyLevel <= 1174 then
            NameMon = "Ship Engineer"
            LevelQuest = 2
            NameQuest = "ShipQuest1"
            CFrameQuest = CFrame.new(1038, 125, 32864)
            CFrameMon = CFrame.new(1198, 125, 32851)
        elseif MyLevel <= 1175 then
            NameMon = "Ship Steward"
            LevelQuest = 1
            NameQuest = "ShipQuest2"
            CFrameQuest = CFrame.new(1038, 125, 32864)
            CFrameMon = CFrame.new(1104, 125, 32739)
 elseif MyLevel <= 1199 then
            NameMon = "Ship Officer"
            LevelQuest = 2
            NameQuest = "ShipQuest2"
            CFrameQuest = CFrame.new(1038, 125, 32864)
            CFrameMon = CFrame.new(1046, 125, 32787)
        elseif MyLevel <= 1249 then
            NameMon = "Arctic Warrior"
            LevelQuest = 1
            NameQuest = "FrostQuest"
            CFrameQuest = CFrame.new(5568, 28, -4825)
            CFrameMon = CFrame.new(5622, 28, -4950)
        elseif MyLevel <= 1299 then
            NameMon = "Snow Lurker"
            LevelQuest = 2
            NameQuest = "FrostQuest"
            CFrameQuest = CFrame.new(5568, 28, -4825)
            CFrameMon = CFrame.new(5400, 28, -4840)
        elseif MyLevel <= 1349 then
            NameMon = "Sea Soldier"
            LevelQuest = 1
            NameQuest = "ForgottenQuest"
            CFrameQuest = CFrame.new(-3052, 17, -10160)
            CFrameMon = CFrame.new(-3187, 20, -9709)
        elseif MyLevel <= 1399 then
            NameMon = "Water Fighter"
            LevelQuest = 2
            NameQuest = "ForgottenQuest"
            CFrameQuest = CFrame.new(-3052, 17, -10160)
            CFrameMon = CFrame.new(-3045, 43, -9770)
        elseif MyLevel <= 1424 then
            NameMon = "Forest Pirate"
            LevelQuest = 1
            NameQuest = "Area3Quest"
            CFrameQuest = CFrame.new(-13519, 332, -751)
            CFrameMon = CFrame.new(-13320, 328, -790)
elseif MyLevel <= 1449 then
            NameMon = "Mythological Pirate"
            LevelQuest = 2
            NameQuest = "Area3Quest"
            CFrameQuest = CFrame.new(-13519, 332, -751)
            CFrameMon = CFrame.new(-13658, 332, -696)
        elseif MyLevel <= 1474 then
            NameMon = "Jungle Pirate"
            LevelQuest = 1
            NameQuest = "DeepForestIsland3"
            CFrameQuest = CFrame.new(-12684, 391, -990)
            CFrameMon = CFrame.new(-12234, 459, -1053)
        elseif MyLevel <= 1499 then
            NameMon = "Musketeer Pirate"
            LevelQuest = 2
            NameQuest = "DeepForestIsland3"
            CFrameQuest = CFrame.new(-12684, 391, -990)
            CFrameMon = CFrame.new(-13294, 519, -762)
        elseif MyLevel <= 1524 then
            NameMon = "Reborn Skeleton"
            LevelQuest = 1
            NameQuest = "HauntedQuest1"
            CFrameQuest = CFrame.new(-9515, 142, 5764)
            CFrameMon = CFrame.new(-9512, 120, 6048)
        elseif MyLevel <= 1574 then
            NameMon = "Living Zombie"
            LevelQuest = 2
            NameQuest = "HauntedQuest1"
            CFrameQuest = CFrame.new(-9515, 142, 5764)
            CFrameMon = CFrame.new(-9760, 110, 6039)
        elseif MyLevel <= 1599 then
            NameMon = "Demonic Soul"
            LevelQuest = 1
            NameQuest = "HauntedQuest2"
            CFrameQuest = CFrame.new(-9515, 172, 5764)
            CFrameMon = CFrame.new(-9507, 176, 6158)
        elseif MyLevel <= 1624 then
            NameMon = "Posessed Mummy"
            LevelQuest = 2
            NameQuest = "HauntedQuest2"
            CFrameQuest = CFrame.new(-9515, 172, 5764)
            CFrameMon = CFrame.new(-9585, 143, 6313)
        elseif MyLevel >= 1625 and World2 then
            -- Teleporte automático para o Terceiro Mundo
            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelDressrosa")
        end
 elseif World3 then
        if MyLevel <= 1574 then
            NameMon = "Pirate Millionaire"
            LevelQuest = 1
            NameQuest = "PiratePortQuest"
            CFrameQuest = CFrame.new(-290, 43, 5581)
            CFrameMon = CFrame.new(-245, 47, 5584)
        elseif MyLevel <= 1599 then
            NameMon = "Pistol Billionaire"
            LevelQuest = 2
            NameQuest = "PiratePortQuest"
            CFrameQuest = CFrame.new(-290, 43, 5581)
            CFrameMon = CFrame.new(-187, 47, 6023)
        elseif MyLevel <= 1624 then
            NameMon = "Dragon Crew Warrior"
            LevelQuest = 1
            NameQuest = "AmazonQuest"
            CFrameQuest = CFrame.new(5448, 602, 752)
            CFrameMon = CFrame.new(5478, 602, 878)
        elseif MyLevel <= 1649 then
            NameMon = "Dragon Crew Archer"
            LevelQuest = 2
            NameQuest = "AmazonQuest"
            CFrameQuest = CFrame.new(5448, 602, 752)
            CFrameMon = CFrame.new(5219, 602, 1149)
        elseif MyLevel <= 1674 then
            NameMon = "Female Islander"
            LevelQuest = 1
            NameQuest = "AmazonQuest2"
            CFrameQuest = CFrame.new(5448, 602, 752)
            CFrameMon = CFrame.new(4956, 602, 722)
        elseif MyLevel <= 1699 then
            NameMon = "Giant Islander"
            LevelQuest = 2
            NameQuest = "AmazonQuest2"
            CFrameQuest = CFrame.new(5448, 602, 752)
            CFrameMon = CFrame.new(4755, 602, 798)
        elseif MyLevel <= 1724 then
            NameMon = "Marine Commodore"
            LevelQuest = 1
            NameQuest = "MarineTreeIsland"
            CFrameQuest = CFrame.new(2178, 29, -6731)
            CFrameMon = CFrame.new(2052, 29, -6496)
        elseif MyLevel <= 1774 then
            NameMon = "Marine Rear Admiral"
            LevelQuest = 2
            NameQuest = "MarineTreeIsland"
            CFrameQuest = CFrame.new(2178, 29, -6731)
            CFrameMon = CFrame.new(2487, 29, -6444)
 elseif World3 then
        if MyLevel <= 1574 then
            NameMon = "Pirate Millionaire"
            LevelQuest = 1
            NameQuest = "PiratePortQuest"
            CFrameQuest = CFrame.new(-290, 43, 5581)
            CFrameMon = CFrame.new(-245, 47, 5584)
        elseif MyLevel <= 1599 then
            NameMon = "Pistol Billionaire"
            LevelQuest = 2
            NameQuest = "PiratePortQuest"
            CFrameQuest = CFrame.new(-290, 43, 5581)
            CFrameMon = CFrame.new(-187, 47, 6023)
        elseif MyLevel <= 1624 then
            NameMon = "Dragon Crew Warrior"
            LevelQuest = 1
            NameQuest = "AmazonQuest"
            CFrameQuest = CFrame.new(5448, 602, 752)
            CFrameMon = CFrame.new(5478, 602, 878)
        elseif MyLevel <= 1649 then
            NameMon = "Dragon Crew Archer"
            LevelQuest = 2
            NameQuest = "AmazonQuest"
            CFrameQuest = CFrame.new(5448, 602, 752)
            CFrameMon = CFrame.new(5219, 602, 1149)
        elseif MyLevel <= 1674 then
            NameMon = "Female Islander"
            LevelQuest = 1
            NameQuest = "AmazonQuest2"
            CFrameQuest = CFrame.new(5448, 602, 752)
            CFrameMon = CFrame.new(4956, 602, 722)
        elseif MyLevel <= 1699 then
            NameMon = "Giant Islander"
            LevelQuest = 2
            NameQuest = "AmazonQuest2"
            CFrameQuest = CFrame.new(5448, 602, 752)
            CFrameMon = CFrame.new(4755, 602, 798)
        elseif MyLevel <= 1724 then
            NameMon = "Marine Commodore"
            LevelQuest = 1
            NameQuest = "MarineTreeIsland"
            CFrameQuest = CFrame.new(2178, 29, -6731)
            CFrameMon = CFrame.new(2052, 29, -6496)
        elseif MyLevel <= 1774 then
            NameMon = "Marine Rear Admiral"
            LevelQuest = 2
            NameQuest = "MarineTreeIsland"
            CFrameQuest = CFrame.new(2178, 29, -6731)
            CFrameMon = CFrame.new(2487, 29, -6444)
        elseif MyLevel <= 1999 then
            NameMon = "Demonic Soul"
            LevelQuest = 1
            NameQuest = "HauntedQuest2"
            CFrameQuest = CFrame.new(-9515, 172, 5764)
            CFrameMon = CFrame.new(-9507, 176, 6158)
        elseif MyLevel <= 2024 then
            NameMon = "Posessed Mummy"
            LevelQuest = 2
            NameQuest = "HauntedQuest2"
            CFrameQuest = CFrame.new(-9515, 172, 5764)
            CFrameMon = CFrame.new(-9585, 143, 6313)
        elseif MyLevel <= 2049 then
            NameMon = "Peanut Scout"
            LevelQuest = 1
            NameQuest = "NutsIslandQuest"
            CFrameQuest = CFrame.new(-2105, 38, -10192)
            CFrameMon = CFrame.new(-2125, 38, -10065)
        elseif MyLevel <= 2074 then
            NameMon = "Peanut President"
            LevelQuest = 2
            NameQuest = "NutsIslandQuest"
            CFrameQuest = CFrame.new(-2105, 38, -10192)
            CFrameMon = CFrame.new(-2125, 38, -10439)
        elseif MyLevel <= 2099 then
            NameMon = "Ice Cream Chef"
            LevelQuest = 1
            NameQuest = "IceCreamIslandQuest"
            CFrameQuest = CFrame.new(-878, 65, -10971)
            CFrameMon = CFrame.new(-872, 65, -10812)
        elseif MyLevel <= 2124 then
            NameMon = "Ice Cream Commander"
            LevelQuest = 2
            NameQuest = "IceCreamIslandQuest"
            CFrameQuest = CFrame.new(-878, 65, -10971)
            CFrameMon = CFrame.new(-1012, 65, -11118)
        elseif MyLevel <= 2149 then
            NameMon = "Cookie Crafter"
            LevelQuest = 1
            NameQuest = "CakeQuest1"
            CFrameQuest = CFrame.new(-2023, 73, -12025)
            CFrameMon = CFrame.new(-2025, 73, -12259)
        elseif MyLevel <= 2199 then
            NameMon = "Cake Guard"
            LevelQuest = 2
            NameQuest = "CakeQuest1"
            CFrameQuest = CFrame.new(-2023, 73, -12025)
            CFrameMon = CFrame.new(-1657, 73, -12229)
        elseif MyLevel <= 2224 then
            NameMon = "Baking Staff"
            LevelQuest = 1
            NameQuest = "CakeQuest2"
            CFrameQuest = CFrame.new(-1928, 129, -12944)
            CFrameMon = CFrame.new(-2063, 129, -12942)
        elseif MyLevel <= 2249 then
            NameMon = "Head Baker"
            LevelQuest = 2
            NameQuest = "CakeQuest2"
            CFrameQuest = CFrame.new(-1928, 129, -12944)
            CFrameMon = CFrame.new(-1879, 129, -13283)
        elseif MyLevel <= 2299 then
            NameMon = "Cocoa Warrior"
            LevelQuest = 1
            NameQuest = "ChocoIslandQuest"
            CFrameQuest = CFrame.new(231, 23, -12191)
            CFrameMon = CFrame.new(166, 13, -12245)
        elseif MyLevel <= 2349 then
            NameMon = "Chocolate Bar Battler"
            LevelQuest = 2
            NameQuest = "ChocoIslandQuest"
            CFrameQuest = CFrame.new(231, 23, -12191)
            CFrameMon = CFrame.new(180, 23, -12396)
        elseif MyLevel <= 2374 then
            NameMon = "Sweet Thief"
            LevelQuest = 1
            NameQuest = "CandyQuest1"
            CFrameQuest = CFrame.new(147, 22, -12427)
            CFrameMon = CFrame.new(66, 23, -12518)
        elseif MyLevel <= 2399 then
            NameMon = "Candy Rebel"
            LevelQuest = 2
            NameQuest = "CandyQuest1"
            CFrameQuest = CFrame.new(147, 22, -12427)
            CFrameMon = CFrame.new(-89, 23, -12461)
        elseif MyLevel <= 2424 then
            NameMon = "Candy Pirate"
            LevelQuest = 1
            NameQuest = "CandyQuest2"
            CFrameQuest = CFrame.new(234, 25, -12700)
            CFrameMon = CFrame.new(98, 24, -12773)
        elseif MyLevel <= 2449 then
            NameMon = "Ice Admiral"
            LevelQuest = 2
            NameQuest = "CandyQuest2"
            CFrameQuest = CFrame.new(234, 25, -12700)
            CFrameMon = CFrame.new(390, 25, -12825)
        elseif MyLevel <= 2474 then
            NameMon = "Cake Warrior"
            LevelQuest = 1
            NameQuest = "CakeIslandQuest"
            CFrameQuest = CFrame.new(-2031, 55, -12589)
            CFrameMon = CFrame.new(-2175, 55, -12561)
        elseif MyLevel <= 2499 then
            NameMon = "Baking Assistant"
            LevelQuest = 2
            NameQuest = "CakeIslandQuest"
            CFrameQuest = CFrame.new(-2031, 55, -12589)
            CFrameMon = CFrame.new(-1934, 55, -12427)
        elseif MyLevel <= 2524 then
            NameMon = "Sweet Commander"
            LevelQuest = 1
            NameQuest = "CakeIsland2Quest"
            CFrameQuest = CFrame.new(-1759, 179, -13065)
            CFrameMon = CFrame.new(-1797, 179, -13246)
        elseif MyLevel >= 2525 then
            NameMon = "Cake Queen"
            LevelQuest = 2
            NameQuest = "CakeIsland2Quest"
            CFrameQuest = CFrame.new(-1759, 179, -13065)
            CFrameMon = CFrame.new(-1641, 179, -13334)
        end
        function ColetarFrutas()
    for _, fruit in pairs(workspace:GetDescendants()) do
        if fruit:IsA("Tool") and fruit:FindFirstChild("Handle") then
            local frutaNome = fruit.Name:lower()
            if frutaNome:match("fruit") or frutaNome:match("blox") then
                pcall(function()
                    game.Players.LocalPlayer.Character.Humanoid:EquipTool(fruit)
                    fruit.Handle.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
                    wait(0.2)
                end)
            end
        end
    end
end
spawn(function()
    while true do
        if _G.AutoFruit then
            ColetarFrutas()
        end
        wait(10) -- verifica a cada 10 segundos
    end
end)
