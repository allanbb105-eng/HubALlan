-- ALLAN HUB – Script com Auto Quest, Auto Farm e Auto Fruit

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
_G.AutoFruit = true

Toggle.MouseButton1Click:Connect(function()
    _G.AutoFarm = not _G.AutoFarm
    Toggle.Text = _G.AutoFarm and "Desativar Allan Hub" or "Ativar Allan Hub"
end)

ModeToggle.MouseButton1Click:Connect(function()
    _G.UseTeleport = not _G.UseTeleport
    ModeToggle.Text = _G.UseTeleport and "Modo: Teleporte" or "Modo: BodyPosition"
end)

function TP(pos)
    if typeof(pos) ~= "CFrame" then return end
    local hrp = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if hrp then hrp.CFrame = pos wait(0.1) end
end

function EquipWeapon(name)
    if not name then return end
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
    local Combat
    pcall(function()
        Combat = require(game.Players.LocalPlayer.PlayerScripts:FindFirstChild("CombatFramework"))
    end)
    if Combat and Combat.activeController and Combat.activeController.equipped then
        for i = 1, 3 do
            pcall(function() Combat.activeController:attack() end)
            wait(0.05)
        end
    else
        SimulateClick()
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

function ColetarFrutas()
    for _, fruit in pairs(workspace:GetDescendants()) do
        if fruit:IsA("Tool") and fruit:FindFirstChild("Handle") then
            local nome = fruit.Name:lower()
            if nome:match("fruit") or nome:match("blox") then
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
            pcall(ColetarFrutas)
        end
        wait(10)
    end
end)

-- Variáveis da missão
NameMon, LevelQuest, NameQuest, CFrameQuest, CFrameMon = nil, nil, nil, nil, nil

function CheckQuest()
    local level = game.Players.LocalPlayer:FindFirstChild("Data") and game.Players.LocalPlayer.Data:FindFirstChild("Level") and game.Players.LocalPlayer.Data.Level.Value or 1
    local id = game.PlaceId
    if id == 2753915549 then -- World1
        if level <= 9 then
            NameMon = "Bandit"
            LevelQuest = 1
            NameQuest = "BanditQuest1"
            CFrameQuest = CFrame.new(1059, 15, 1550)
            CFrameMon = CFrame.new(1046, 27, 1560)
        elseif level <= 14 then
            NameMon = "Monkey"
            LevelQuest = 1
            NameQuest = "JungleQuest"
            CFrameQuest = CFrame.new(-1598, 35, 153)
            CFrameMon = CFrame.new(-1448, 67, 11)
        end
    elseif id == 4442272183 then -- World2
        if level <= 724 then
            NameMon = "Raider"
            LevelQuest = 1
            NameQuest = "Area1Quest"
            CFrameQuest = CFrame.new(-429, 71, 1836)
            CFrameMon = CFrame.new(-728, 52, 2345)
        elseif level >= 1625 then
            pcall(function()
                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelDressrosa")
            end)
        end
    elseif id == 7449423635 then -- World3
        if level <= 1574 then
            NameMon = "Pirate Millionaire"
            LevelQuest = 1
            NameQuest = "PiratePortQuest"
            CFrameQuest = CFrame.new(-290, 43, 5581)
            CFrameMon = CFrame.new(-245, 47, 5584)
        elseif level >= 2525 then
            NameMon = "Cake Queen"
            LevelQuest = 2
            NameQuest = "CakeIsland2Quest"
            CFrameQuest = CFrame.new(-1759, 179, -13065)
            CFrameMon = CFrame.new(-1641, 179, -13334)
        end
    end
end

function AceitarMissao()
    pcall(function()
        if NameQuest and LevelQuest then
            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
            wait(0.1)
            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", NameQuest, LevelQuest)
        end
    end)
end

spawn(function()
    while true do
        if _G.AutoFarm then
            pcall(function()
                CheckQuest()
                AceitarMissao()
                EquipWeapon(_G.Weapon)
                local mob = nil
                for _, enemy in pairs(workspace.Enemies:GetChildren()) do
                    if enemy.Name == NameMon and enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 and enemy:FindFirstChild("HumanoidRootPart") then
                        mob = enemy.HumanoidRootPart
                        break
                    end
                end
                if mob then
                    if _G.UseTeleport then
                        TP(mob.CFrame + Vector3.new(0, 3, 0))
                    else
                        StickToMob(mob)
                    end
                    Attack()
                else
                    ClearStick()
                    TP(CFrameMon)
                end
            end)
        else
            ClearStick()
        end
        wait(0.25)
    end
end)
