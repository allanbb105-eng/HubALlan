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
