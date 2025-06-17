-- ALLAN HUB vFinal – Corrigido e Estável (Auto Farm + Auto Quest + Auto Fruit)

local gui = Instance.new("ScreenGui", game:GetService("CoreGui"))
gui.Name = "AllanHub"

local Toggle = Instance.new("TextButton", gui)
Toggle.Position = UDim2.new(0, 10, 0, 100)
Toggle.Size = UDim2.new(0, 150, 0, 40)
Toggle.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Toggle.Font = Enum.Font.SourceSansBold
Toggle.Text = "Ativar Allan Hub"
Toggle.TextSize = 20
Toggle.TextColor3 = Color3.new(1, 1, 1)

local ModeToggle = Instance.new("TextButton", gui)
ModeToggle.Position = UDim2.new(0, 10, 0, 150)
ModeToggle.Size = UDim2.new(0, 150, 0, 40)
ModeToggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ModeToggle.Font = Enum.Font.SourceSansBold
ModeToggle.Text = "Modo: BodyPosition"
ModeToggle.TextSize = 18
ModeToggle.TextColor3 = Color3.new(1, 1, 1)

-- Variáveis de controle
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

-- Utilitários
function TP(pos)
    local hrp = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if hrp and typeof(pos) == "CFrame" then hrp.CFrame = pos end
end

function EquipWeapon(name)
    local tool = game.Players.LocalPlayer.Backpack:FindFirstChild(name)
    if tool then
        game.Players.LocalPlayer.Character.Humanoid:EquipTool(tool)
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
        for _ = 1, 3 do
            pcall(function() Combat.activeController:attack() end)
            wait(0.05)
        end
    else
        SimulateClick()
    end
end

function StickToMob(target)
    local hrp = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
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
    local hrp = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        local bp = hrp:FindFirstChild("AllanStick")
        if bp then bp:Destroy() end
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
                end)
            end
        end
    end
end

-- AutoFruit loop
spawn(function()
    while task.wait(10) do
        if _G.AutoFruit then pcall(ColetarFrutas) end
    end
end)

-- AutoQuest
NameMon, NameQuest, LevelQuest, CFrameQuest, CFrameMon = nil, nil, nil, nil, nil

function CheckQuest()
    local level = game.Players.LocalPlayer:FindFirstChild("Data") and game.Players.LocalPlayer.Data:FindFirstChild("Level").Value or 1
    local place = game.PlaceId
    if place == 2753915549 then
        if level <= 9 then
            NameMon = "Bandit"
            NameQuest = "BanditQuest1"
            LevelQuest = 1
            CFrameQuest = CFrame.new(1059, 15, 1550)
            CFrameMon = CFrame.new(1046, 27, 1560)
        elseif level <= 14 then
            NameMon = "Monkey"
            NameQuest = "JungleQuest"
            LevelQuest = 1
            CFrameQuest = CFrame.new(-1598, 35, 153)
            CFrameMon = CFrame.new(-1448, 67, 11)
        end
    elseif place == 4442272183 then
        if level <= 724 then
            NameMon = "Raider"
            NameQuest = "Area1Quest"
            LevelQuest = 1
            CFrameQuest = CFrame.new(-429, 71, 1836)
            CFrameMon = CFrame.new(-728, 52, 2345)
        elseif level >= 1625 then
            pcall(function()
                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelDressrosa")
            end)
        end
    elseif place == 7449423635 then
        if level <= 1574 then
            NameMon = "Pirate Millionaire"
            NameQuest = "PiratePortQuest"
            LevelQuest = 1
            CFrameQuest = CFrame.new(-290, 43, 5581)
            CFrameMon = CFrame.new(-245, 47, 5584)
        elseif level >= 2525 then
            NameMon = "Cake Queen"
            NameQuest = "CakeIsland2Quest"
            LevelQuest = 2
            CFrameQuest = CFrame.new(-1759, 179, -13065)
            CFrameMon = CFrame.new(-1641, 179, -13334)
        end
    end
end

function AceitarMissao()
    pcall(function()
        if NameQuest and LevelQuest then
            local Rep = game:GetService("ReplicatedStorage")
            Rep.Remotes.CommF_:InvokeServer("AbandonQuest")
            wait(0.2)
            Rep.Remotes.CommF_:InvokeServer("StartQuest", NameQuest, LevelQuest)
        end
    end)
end

-- AutoFarm loop
spawn(function()
    while wait(0.2) do
        if _G.AutoFarm then
            pcall(function()
                CheckQuest()
                AceitarMissao()
                EquipWeapon(_G.Weapon)
                local target
                for _, enemy in pairs(workspace.Enemies:GetChildren()) do
                    if enemy.Name == NameMon and enemy:FindFirstChild("HumanoidRootPart") then
                        local humanoid = enemy:FindFirstChild("Humanoid")
                        if humanoid and humanoid.Health > 0 then
                            target = enemy.HumanoidRootPart
                            break
                        end
                    end
                end
                if target then
                    if _G.UseTeleport then
                        TP(target.CFrame + Vector3.new(0, 3, 0))
                    else
                        StickToMob(target)
                    end
                    Attack()
                elseif CFrameMon then
                    ClearStick()
                    TP(CFrameMon)
                end
            end)
        else
            ClearStick()
        end
    end
end)
