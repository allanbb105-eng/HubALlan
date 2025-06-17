-- Interface do Allan Hub
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
Toggle.MouseButton1Click:Connect(function()
    _G.AutoFarm = not _G.AutoFarm
    Toggle.Text = _G.AutoFarm and "Desativar Allan Hub" or "Ativar Allan Hub"
end)
ModeToggle.MouseButton1Click:Connect(function()
    _G.UseTeleport = not _G.UseTeleport
    ModeToggle.Text = _G.UseTeleport and "Modo: Teleporte" or "Modo: BodyPosition"
end)

-- Configurações principais
_G.Weapon = "Combat"

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

function FindMob(name)
    for _, mob in pairs(workspace.Enemies:GetChildren()) do
        if mob.Name:match(name) and mob:FindFirstChild("Humanoid") and mob.Humanoid.Health > 0 then
            mob:WaitForChild("HumanoidRootPart")
            return mob
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

-- NOVA FUNÇÃO: Magnetismo!
function MagnetMobs(center)
    for _, mob in pairs(workspace.Enemies:GetChildren()) do
        if mob:FindFirstChild("HumanoidRootPart") and mob:FindFirstChild("Humanoid") and mob.Humanoid.Health > 0 then
            pcall(function()
                mob.HumanoidRootPart.CFrame = CFrame.new(center + Vector3.new(math.random(-3,3), 0, math.random(-3,3)))
            end)
        end
    end
end

ffunction CheckQuest()
    MyLevel = game.Players.LocalPlayer.Data.Level.Value
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
            {Level = 700, Mob = "Raider", Quest = "Area1Quest", QuestLevel = 1, CFrameQuest = CFrame.new(-429, 71, 1836), CFrameMon = CFrame.new(-728, 52, 2345)},
        },
        [7449423635] = {
            {Level = 1500, Mob = "Pirate Millionaire", Quest = "PiratePortQuest", QuestLevel = 1, CFrameQuest = CFrame.new(-290, 42, 5581), CFrameMon = CFrame.new(-246, 47, 5584)},
        }
    }
    local region = Worlds[placeId]
    if not region then return end
    for i = #region, 1, -1 do
        local s = region[i]
        if level >= s.Level then
            NameMon = s.Mob
            NameQuest = s.Quest
            LevelQuest = s.QuestLevel
            CFrameQuest = s.CFrameQuest
            CFrameMon = s.CFrameMon
            break
        end
    end
end

-- LOOP PRINCIPAL COM ÍMÃ ATIVADO
spawn(function()
    while true do
        if _G.AutoFarm then
            pcall(function()
                CheckQuest()
                if not game.Players.LocalPlayer.PlayerGui.Main.Quest.Visible then
                    ClearStick()
                    TP(CFrameQuest)
                    wait(1)
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", NameQuest, LevelQuest)
                else
                    local mob = FindMob(NameMon)
                    if mob then
                        EquipWeapon(_G.Weapon)
                        if _G.UseTeleport then
                            TP(mob.HumanoidRootPart.CFrame * CFrame.new(0, 1.5, 0))
                        else
                            StickToMob(mob.HumanoidRootPart)
                        end
                        MagnetMobs(game.Players.LocalPlayer.Character.HumanoidRootPart.Position) -- 🧲 Magnetismo aqui
                        wait(0.2)
                        Attack()
                        if mob.Humanoid.Health <= 0 then
                            ClearStick()
                        end
                    else
                        ClearStick()
                        TP(CFrameMon)
                        wait(1)
                    end
                end
            end)
        end
        wait(0.05)
    end
end)
