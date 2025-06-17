-- Allan Hub Interface
local ScreenGui = Instance.new("ScreenGui")
local Toggle = Instance.new("TextButton")

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

_G.AutoFarm = false
Toggle.MouseButton1Click:Connect(function()
    _G.AutoFarm = not _G.AutoFarm
    Toggle.Text = _G.AutoFarm and "Desativar Allan Hub" or "Ativar Allan Hub"
end)

-- Configurações
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
            local blade = controller.blades[1]
            if blade then
                for i = 1, 3 do
                    controller:attack()
                    wait(0.05)
                end
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
    local char = game.Players.LocalPlayer.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    if not (hrp and target) then return end

    local bodyPos = hrp:FindFirstChild("AllanStick") or Instance.new("BodyPosition")
    bodyPos.Name = "AllanStick"
    bodyPos.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bodyPos.Position = target.Position + Vector3.new(0, 0.8, 0)
    bodyPos.P = 5000
    bodyPos.D = 500
    bodyPos.Parent = hrp
end

function ClearStick()
    local hrp = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        local stick = hrp:FindFirstChild("AllanStick")
        if stick then stick:Destroy() end
    end
end

function CheckQuest()
    local level = game.Players.LocalPlayer.Data.Level.Value
    local placeId = game.PlaceId
    local Worlds = {
        [2753915549] = {
            {Level = 1, Mob = "Bandit", Quest = "BanditQuest1", QuestLevel = 1, CFrameQuest = CFrame.new(1059, 16, 1544), CFrameMon = CFrame.new(1046, 27, 1561)},
        },
        [4442272183] = {
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
                        StickToMob(mob.HumanoidRootPart)
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
