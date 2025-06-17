-- 🌀 Interface Gráfica
local gui = Instance.new("ScreenGui", game:GetService("CoreGui"))
gui.Name = "AllanHub"

local toggleBtn = Instance.new("TextButton", gui)
toggleBtn.Position = UDim2.new(0, 10, 0, 100)
toggleBtn.Size = UDim2.new(0, 150, 0, 40)
toggleBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
toggleBtn.Font = Enum.Font.SourceSansBold
toggleBtn.Text = "Ativar Allan Hub"
toggleBtn.TextSize = 20
toggleBtn.TextColor3 = Color3.new(1, 1, 1)

local modeBtn = Instance.new("TextButton", gui)
modeBtn.Position = UDim2.new(0, 10, 0, 150)
modeBtn.Size = UDim2.new(0, 150, 0, 40)
modeBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
modeBtn.Font = Enum.Font.SourceSansBold
modeBtn.Text = "Modo: BodyPosition"
modeBtn.TextSize = 18
modeBtn.TextColor3 = Color3.new(1, 1, 1)

-- 🔧 Variáveis principais
_G.AutoFarm = false
_G.UseTeleport = false
_G.Weapon = "Combat"
_G.AutoFruit = true

-- 🌐 Botões
toggleBtn.MouseButton1Click:Connect(function()
    _G.AutoFarm = not _G.AutoFarm
    toggleBtn.Text = _G.AutoFarm and "Desativar Allan Hub" or "Ativar Allan Hub"
end)

modeBtn.MouseButton1Click:Connect(function()
    _G.UseTeleport = not _G.UseTeleport
    modeBtn.Text = _G.UseTeleport and "Modo: Teleporte" or "Modo: BodyPosition"
end)

-- 📦 Utilitários
function TP(cframe)
    local hrp = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if hrp and typeof(cframe) == "CFrame" then
        hrp.CFrame = cframe
    end
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
    bp.Position = target.Position + Vector3.new(0, 1, 0)
    bp.P = 5000
    bp.D = 500
    bp.Parent = hrp
end

function ClearStick()
    local hrp = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
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
                end)
            end
        end
    end
end

-- 🍍 Auto Fruit
spawn(function()
    while wait(10) do
        if _G.AutoFruit then
            pcall(ColetarFrutas)
        end
    end
end)
-- 📈 Auto distribuir pontos de status (Stats)
_G.AutoStats = true

spawn(function()
	while wait(1) do
		if _G.AutoStats then
			pcall(function()
				local stats = game.Players.LocalPlayer.Data.Stats
				if stats then
					local melee = stats.Melee.Level.Value
					local defense = stats.Defense.Level.Value
					local sword = stats.Sword.Level.Value
					if melee < 2400 then
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint", "Melee")
					elseif defense < 2400 then
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint", "Defense")
					elseif sword < 2400 then
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint", "Sword")
					end
				end
			end)
		end
	end
end)

-- 🧭 Variáveis de missão
NameMon, NameQuest, LevelQuest, CFrameQuest, CFrameMon = nil, nil, nil, nil, nil

function CheckQuest()
    local level = game.Players.LocalPlayer.Data.Level.Value
    local place = game.PlaceId
    if place == 2753915549 then
        if level <= 14 then
            NameMon = "Bandit"
            NameQuest = "BanditQuest1"
            LevelQuest = 1
            CFrameQuest = CFrame.new(1059, 15, 1550)
            CFrameMon = CFrame.new(1046, 27, 1560)
        elseif level <= 29 then
            NameMon = "Monkey"
            NameQuest = "JungleQuest"
            LevelQuest = 1
            CFrameQuest = CFrame.new(-1598, 35, 153)
            CFrameMon = CFrame.new(-1448, 67, 11)
        end
    end
end

-- ✅ Corrigido: só aceita missão se necessário
function AceitarMissao()
    local Rep = game:GetService("ReplicatedStorage")
    local Player = game.Players.LocalPlayer
    local gui = Player:FindFirstChild("PlayerGui")
    local current = gui and gui:FindFirstChild("QuestGUI") and gui.QuestGUI:FindFirstChild("Title")
    if not current or not current.Text:find(NameQuest) then
        pcall(function()
            Rep.Remotes.CommF_:InvokeServer("AbandonQuest")
            wait(4)
            Rep.Remotes.CommF_:InvokeServer("StartQuest", NameQuest, LevelQuest)
        end)
    end
end

-- 🔁 AutoFarm principal
spawn(function()
    while wait(0.25) do
        if _G.AutoFarm then
            pcall(function()
                CheckQuest()
                AceitarMissao()
                EquipWeapon(_G.Weapon)
                local alvo = nil
                for _, enemy in pairs(workspace.Enemies:GetChildren()) do
                    if enemy.Name == NameMon and enemy:FindFirstChild("HumanoidRootPart") then
                        local humanoid = enemy:FindFirstChild("Humanoid")
                        if humanoid and humanoid.Health > 0 then
                            alvo = enemy.HumanoidRootPart
                            break
                        end
                    end
                end
                if alvo then
                    if _G.UseTeleport then
                        TP(alvo.CFrame + Vector3.new(0, 3, 0))
                    else
                        StickToMob(alvo)
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
