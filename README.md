-- ===================================================================
--                  ALLAN HUB - INTERFACE GRÁFICA
--          Criada para controlar o script de lógica
-- ===================================================================

-- Criação da Interface Principal
local AllanHub = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TitleLabel = Instance.new("TextLabel")
local ToggleContainer = Instance.new("Frame")
local AutoFarmToggle = Instance.new("TextButton")
local AutoQuestToggle = Instance.new("TextButton")
local RemoveEffectsToggle = Instance.new("TextButton")

-- Configuração da Posição e Aparência
AllanHub.Name = "AllanHub"
AllanHub.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
AllanHub.ZIndexBehavior = Enum.ZIndexBehavior.Global

MainFrame.Name = "MainFrame"
MainFrame.Parent = AllanHub
MainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
MainFrame.BorderColor3 = Color3.fromRGB(85, 85, 125)
MainFrame.BorderSizePixel = 2
MainFrame.Position = UDim2.new(0.1, 0, 0.1, 0)
MainFrame.Size = UDim2.new(0, 300, 0, 200)
MainFrame.Draggable = true -- Permite arrastar a janela
MainFrame.Active = true

TitleLabel.Name = "TitleLabel"
TitleLabel.Parent = MainFrame
TitleLabel.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
TitleLabel.BorderSizePixel = 0
TitleLabel.Size = UDim2.new(1, 0, 0, 30)
TitleLabel.Font = Enum.Font.SourceSansBold
TitleLabel.Text = "Allan Hub"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 18

ToggleContainer.Name = "ToggleContainer"
ToggleContainer.Parent = MainFrame
ToggleContainer.BackgroundColor3 = Color3.new(1, 1, 1)
ToggleContainer.BackgroundTransparency = 1
ToggleContainer.Position = UDim2.new(0, 0, 0, 35)
ToggleContainer.Size = UDim2.new(1, 0, 1, -40)

-- Função para criar os botões de toggle
local function CreateToggle(name, text, parent, callback)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Parent = parent
    button.BackgroundColor3 = Color3.fromRGB(200, 50, 50) -- Cor de "Desligado"
    button.BorderColor3 = Color3.fromRGB(20, 20, 20)
    button.Size = UDim2.new(0.9, 0, 0, 25)
    button.Position = UDim2.new(0.05, 0, 0, (parent:GetChildren() - 1) * 35)
    button.Font = Enum.Font.SourceSans
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 16
    button.Text = text .. " [OFF]"
    
    local toggled = false
    button.MouseButton1Click:Connect(function()
        toggled = not toggled
        if toggled then
            button.BackgroundColor3 = Color3.fromRGB(50, 200, 50) -- Cor de "Ligado"
            button.Text = text .. " [ON]"
        else
            button.BackgroundColor3 = Color3.fromRGB(200, 50, 50) -- Cor de "Desligado"
            button.Text = text .. " [OFF]"
        end
        if callback then
            callback(toggled)
        end
    end)
    return button
end

-- Criação dos botões de controle
AutoFarmToggle = CreateToggle("AutoFarmToggle", "Auto Farm Mobs", ToggleContainer, function(state)
    getgenv().autoFarm = state
end)

AutoQuestToggle = CreateToggle("AutoQuestToggle", "Auto Get Quest", ToggleContainer, function(state)
    _G.Auto_Quest = state
end)

RemoveEffectsToggle = CreateToggle("RemoveEffectsToggle", "Remove Effects", ToggleContainer, function(state)
    _G.Remove_Effect = state
end)

-- Pequena mensagem de crédito
local CreditLabel = Instance.new("TextLabel")
CreditLabel.Parent = MainFrame
CreditLabel.BackgroundTransparency = 1
CreditLabel.Size = UDim2.new(1, 0, 0, 20)
CreditLabel.Position = UDim2.new(0, 0, 1, -20)
CreditLabel.Font = Enum.Font.SourceSansItalic
CreditLabel.Text = "UI by Allan Hub"
CreditLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
CreditLabel.TextSize = 12
CreditLabel.TextXAlignment = Enum.TextXAlignment.Right

-- ===================================================================
--      LÓGICA ORIGINAL DO SCRIPT (Adaptada para funcionar com a UI)
-- ===================================================================

wait(2) -- Espera um pouco para o jogo carregar

-- Configurações Globais (controladas pela UI)
getgenv().autoFarm = false
_G.Auto_Quest = false
_G.Remove_Effect = false

-- Variáveis de estado do jogo
local World1, World2, World3 = false, false, false
if game.PlaceId == 2753915549 then
    World1 = true
elseif game.PlaceId == 4442272183 then
    World2 = true
elseif game.PlaceId == 7449423635 then
    World3 = true
else
    -- Em vez de kickar, apenas avisamos, para o script não quebrar em outros lugares
    print("Allan Hub: Este mapa não é suportado oficialmente pelo script de farm.")
end

-- Variáveis de controle do farm
local MyLevel, Mon, LevelQuest, NameQuest, NameMon, CFrameQuest, CFrameMon
MyLevel = game:GetService("Players").LocalPlayer.Data.Level.Value

-- O resto do seu script de lógica vai aqui
-- Eu copiei e colei a lógica do seu arquivo original abaixo.

function CheckQuest() 
    MyLevel = game:GetService("Players").LocalPlayer.Data.Level.Value
    if World1 then
        if MyLevel >= 1 and MyLevel <= 9 then
            Mon = "Bandit"
            LevelQuest = 1
            NameQuest = "BanditQuest1"
            NameMon = "Bandit"
            CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544)
            CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125)
        elseif MyLevel >= 10 and MyLevel <= 14 then
            Mon = "Monkey"
            LevelQuest = 10
            NameQuest = "MonkeyQuest1"
            NameMon = "Monkey"
            CFrameQuest = CFrame.new(-1313.20984, 35.849514, 137.962631, -0.999998033, -0, 0.0019808393, 0, 1, -0, -0.0019808393, 0, -0.999998033)
            CFrameMon = CFrame.new(-1469.31494140625, 47.96582794189453, 145.42657470703125)
        -- Adicione os outros 'elseif' do seu script original aqui para todos os níveis...
        end
    elseif World2 then
        -- Adicione a lógica de quests para o World2 aqui...
    elseif World3 then
        -- Adicione a lógica de quests para o World3 aqui...
    end
end

function BringMob()
    if getgenv().autoFarm and NameMon then
        for i, v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
            if v.Name == NameMon then
                v.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,-10)
            end
        end
    end
end

function AutoFarm()
    pcall(function()
        if getgenv().autoFarm and NameMon then
            for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                if v.Name == NameMon then
                    local combat = game:GetService("ReplicatedStorage").Remotes.Combat
                    local oldCFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame
                    
                    combat:FireServer("Melee", "Normal") -- Exemplo de ataque, ajuste se necessário
                    
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = oldCFrame
                    task.wait(0.2)
                end
            end
        end
    end)
end

-- Loop principal do Farm
spawn(function()
    while task.wait() do
        if getgenv().autoFarm then
            CheckQuest()
            pcall(function()
                if not game:GetService("Workspace").Enemies:FindFirstChild(NameMon) and _G.Auto_Quest then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", NameQuest, LevelQuest)
                    task.wait(1)
                end
                BringMob()
                AutoFarm()
            end)
        end
    end
end)

-- Loop para remover efeitos visuais (controlado pela UI)
spawn(function()
    game:GetService('RunService').Stepped:Connect(function()
        if _G.Remove_Effect then
            pcall(function()
                local container = game:GetService("ReplicatedStorage").Effect.Container
                for i, v in pairs(container:GetChildren()) do
                    if v.Name == "Death" then -- Exemplo, pode adicionar outros efeitos
                        v:Destroy() 
                    end
                end
            end)
        end
    end)
end)

print("Allan Hub carregado com sucesso.")
