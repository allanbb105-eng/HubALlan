-- Interface simples Allan Hub
local ScreenGui = Instance.new("ScreenGui")
local Toggle = Instance.new("TextButton")

ScreenGui.Name = "AllanHub"
ScreenGui.Parent = game.CoreGui

Toggle.Name = "AutoFarmToggle"
Toggle.Parent = ScreenGui
Toggle.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Toggle.BorderSizePixel = 2
Toggle.Position = UDim2.new(0, 10, 0, 100)
Toggle.Size = UDim2.new(0, 140, 0, 40)
Toggle.Font = Enum.Font.SourceSansBold
Toggle.Text = "Ativar Allan Hub"
Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
Toggle.TextSize = 20

_G.AutoFarm = false
Toggle.MouseButton1Click:Connect(function()
    _G.AutoFarm = not _G.AutoFarm
    Toggle.Text = _G.AutoFarm and "Desativar Allan Hub" or "Ativar Allan Hub"
end)

-- Script funcional com sistema completo
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

    local Worlds = {
        [2753915549] = { -- First Sea
            {Level = 1, Mob = "Bandit", Quest = "BanditQuest1", QuestLevel = 1, CFrameQuest = CFrame.new(1059, 16, 1544), CFrameMon = CFrame.new(1046, 27, 1561)},
            -- Adicione mais estágios conforme necessário
        },
        [4442272183] = { -- Second Sea
            {Level = 700, Mob = "Raider", Quest = "Area1Quest", QuestLevel = 1, CFrameQuest = CFrame.new(-429, 71, 1836), CFrameMon = CFrame.new(-728, 52, 2345)},
            -- Adicione mais estágios conforme necessário
        },
        [7449423635] = { -- Third Sea
            {Level = 1500, Mob = "Pirate Millionaire", Quest = "PiratePortQuest", QuestLevel = 1, CFrameQuest = CFrame.new(-290, 42, 5581), CFrameMon = CFrame.new(-246, 47, 5584)},
            -- Adicione mais estágios conforme necessário
        }
    }

    local region = Worlds[placeId]
    if not region then return end

    for i = #region, 1, -1 do
        local stage = region[i]
        if level >= stage.Level then
            NameMon = stage.Mob
            NameQuest = stage.Quest
            LevelQuest = stage.QuestLevel
            CFrameQuest = stage.CFrameQuest
            CFrameMon = stage.CFrameMon
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
        end
        wait()
    end
end)
