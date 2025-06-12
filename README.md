local player = game:GetService("Players").LocalPlayer
local coreGui = game:GetService("CoreGui")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AutoFarmGUI_Detailed"
ScreenGui.Parent = coreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 250, 0, 420)
MainFrame.Position = UDim2.new(0.5, -125, 0.5, -210)
MainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MainFrame.BorderSizePixel = 0
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "TitleLabel"
TitleLabel.Size = UDim2.new(1, 0, 0, 30)
TitleLabel.Position = UDim2.new(0, 0, 0, 0)
TitleLabel.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
TitleLabel.Text = "⚡ Auto Farm Blox Fruits ⚡"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.Font = Enum.Font.SourceSansBold
TitleLabel.TextSize = 18
TitleLabel.BorderSizePixel = 0
TitleLabel.Parent = MainFrame

local StatusLabel = Instance.new("TextLabel")
StatusLabel.Name = "StatusLabel"
StatusLabel.Size = UDim2.new(1, 0, 0, 20)
StatusLabel.Position = UDim2.new(0, 0, 0, 30)
StatusLabel.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
StatusLabel.Text = "Status: Inativo"
StatusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
StatusLabel.Font = Enum.Font.SourceSans
StatusLabel.TextSize = 14
StatusLabel.BorderSizePixel = 0
StatusLabel.Parent = MainFrame

local function updateStatusText(text)
    StatusLabel.Text = text
end

local function updateToggleButton(button, value)
    if value then
        button.Text = button.Name .. ": ON"
        button.BackgroundColor3 = Color3.fromRGB(70, 150, 70)
    else
        button.Text = button.Name .. ": OFF"
        button.BackgroundColor3 = Color3.fromRGB(150, 70, 70)
    end
end

-- Estas variáveis globais (_G) não existirão neste teste simplificado,
-- então vamos defini-las como 'false' temporariamente para que o script não quebre.
_G = _G or {} -- Garante que _G exista
_G.AutoQuest = false
_G.AutoAttack = false
_G.AutoClick = false
_G.AutoClickDelay = 0.05
_G.CurrentFarmActive = false
_G.FarmType = nil


-- Botão Toggle Auto Quest
local AutoQuestButton = Instance.new("TextButton")
AutoQuestButton.Name = "AutoQuest"
AutoQuestButton.Size = UDim2.new(0.9, 0, 0, 25)
AutoQuestButton.Position = UDim2.new(0.05, 0, 0, 60)
AutoQuestButton.Font = Enum.Font.SourceSansBold
AutoQuestButton.TextSize = 14
AutoQuestButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoQuestButton.BorderSizePixel = 0
AutoQuestButton.Parent = MainFrame
updateToggleButton(AutoQuestButton, _G.AutoQuest)

AutoQuestButton.MouseButton1Click:Connect(function()
    _G.AutoQuest = not _G.AutoQuest
    updateToggleButton(AutoQuestButton, _G.AutoQuest)
    print("[Controle] AutoQuest agora está: " .. tostring(_G.AutoQuest))
end)

-- Botão Toggle Auto Attack (afeta o farm de quests)
local AutoAttackButton = Instance.new("TextButton")
AutoAttackButton.Name = "AutoAttack"
AutoAttackButton.Size = UDim2.new(0.9, 0, 0, 25)
AutoAttackButton.Position = UDim2.new(0.05, 0, 0, 90)
AutoAttackButton.Font = Enum.Font.SourceSansBold
AutoAttackButton.TextSize = 14
AutoAttackButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoAttackButton.BorderSizePixel = 0
AutoAttackButton.Parent = MainFrame
updateToggleButton(AutoAttackButton, _G.AutoAttack)

AutoAttackButton.MouseButton1Click:Connect(function()
    _G.AutoAttack = not _G.AutoAttack
    updateToggleButton(AutoAttackButton, _G.AutoAttack)
    print("[Controle] AutoAttack agora está: " .. tostring(_G.AutoAttack))
end)

-- Separador visual
local Separator1 = Instance.new("Frame")
Separator1.Name = "Separator1"
Separator1.Size = UDim2.new(1, 0, 0, 2)
Separator1.Position = UDim2.new(0, 0, 0, 120)
Separator1.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
Separator1.Parent = MainFrame

-- Título para a seção de Auto Farm de Níveis
local LevelFarmLabel = Instance.new("TextLabel")
LevelFarmLabel.Name = "LevelFarmLabel"
LevelFarmLabel.Size = UDim2.new(1, 0, 0, 20)
LevelFarmLabel.Position = UDim2.new(0, 0, 0, 125)
LevelFarmLabel.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
LevelFarmLabel.Text = "--- Auto Farm por Nível ---"
LevelFarmLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
LevelFarmLabel.Font = Enum.Font.SourceSansBold
LevelFarmLabel.TextSize = 14
LevelFarmLabel.BorderSizePixel = 0
LevelFarmLabel.Parent = MainFrame

-- ScrollFrame para os botões de níveis
local LevelButtonsFrame = Instance.new("ScrollingFrame")
LevelButtonsFrame.Name = "LevelButtonsFrame"
LevelButtonsFrame.Size = UDim2.new(1, 0, 0.4, 0)
LevelButtonsFrame.Position = UDim2.new(0, 0, 0, 148)
LevelButtonsFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
LevelButtonsFrame.BorderSizePixel = 0
LevelButtonsFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
LevelButtonsFrame.VerticalScrollBarInset = Enum.ScrollBarInset.Always
LevelButtonsFrame.ScrollingDirection = Enum.ScrollingDirection.Y
LevelButtonsFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Name = "QuestButtonLayout"
UIListLayout.Padding = UDim.new(0, 5)
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIListLayout.VerticalAlignment = Enum.VerticalAlignment.Top
UIListLayout.Parent = LevelButtonsFrame

-- Para o teste, vamos criar um QuestDefinitions simplificado e fixo, sem World1/2/3
local QuestDefinitions = {
    Bandit = {
        FarmName = "Bandit Farm",
        MinLevel = 1,
        MaxLevel = 9,
        Mon = "Bandit",
        NameQuest = "Bandit Quest",
        NameMon = "Bandit",
        NPCName = "Bandit Quest Giver",
        MobsToKill = 5,
        CFrameQuest = CFrame.new(0,0,0), -- Não importa neste teste, pois não há lógica de farm
        CFrameMon = CFrame.new(0,0,0)
    },
    TestQuest = { -- Apenas para garantir que um botão apareça
        FarmName = "Test Farm",
        MinLevel = 100,
        MaxLevel = 109,
        Mon = "TestMob",
        NameQuest = "Test Quest",
        NameMon = "TestMob",
        NPCName = "TestQuestGiver",
        MobsToKill = 5,
        CFrameQuest = CFrame.new(0,0,0),
        CFrameMon = CFrame.new(0,0,0)
    }
}

local function createQuestButton(questName, questData)
    local button = Instance.new("TextButton")
    button.Name = questData.FarmName or questName .. "FarmButton"
    button.Size = UDim2.new(0.9, 0, 0, 30)
    button.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    button.Text = "Farm " .. questData.NameQuest .. " (Lvl " .. questData.MinLevel .. "-" .. questData.MaxLevel .. ")"
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 14
    button.BorderSizePixel = 0
    button.Parent = LevelButtonsFrame

    button.MouseButton1Click:Connect(function()
        print("Botão " .. button.Text .. " clicado! (Nenhuma lógica de farm real neste teste)")
    end)
    return button
end

local currentYOffset = 0
local buttonHeight = 30
local padding = 5

for questName, questData in pairs(QuestDefinitions) do
    -- No teste, não há verificação de World1/2/3 para garantir que os botões apareçam
    createQuestButton(questName, questData)
    currentYOffset = currentYOffset + buttonHeight + padding
end
LevelButtonsFrame.CanvasSize = UDim2.new(0, 0, 0, currentYOffset)


-- Separador visual para Auto Click
local Separator2 = Instance.new("Frame")
Separator2.Name = "Separator2"
Separator2.Size = UDim2.new(1, 0, 0, 2)
Separator2.Position = UDim2.new(0, 0, 0, LevelButtonsFrame.Position.Y.Offset + LevelButtonsFrame.Size.Y.Offset + 5)
Separator2.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
Separator2.Parent = MainFrame

-- Título para a seção de Auto Click
local AutoClickLabel = Instance.new("TextLabel")
AutoClickLabel.Name = "AutoClickLabel"
AutoClickLabel.Size = UDim2.new(1, 0, 0, 20)
AutoClickLabel.Position = UDim2.new(0, 0, 0, Separator2.Position.Y.Offset + Separator2.Size.Y.Offset + 3)
AutoClickLabel.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
AutoClickLabel.Text = "--- Auto Click ---"
AutoClickLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoClickLabel.Font = Enum.Font.SourceSansBold
AutoClickLabel.TextSize = 14
AutoClickLabel.BorderSizePixel = 0
AutoClickLabel.Parent = MainFrame

-- Botão Toggle Auto Click
local ToggleAutoClickButton = Instance.new("TextButton")
ToggleAutoClickButton.Name = "AutoClick"
ToggleAutoClickButton.Size = UDim2.new(0.9, 0, 0, 25)
ToggleAutoClickButton.Position = UDim2.new(0.05, 0, 0, AutoClickLabel.Position.Y.Offset + AutoClickLabel.Size.Y.Offset + 5)
ToggleAutoClickButton.Font = Enum.Font.SourceSansBold
ToggleAutoClickButton.TextSize = 14
ToggleAutoClickButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleAutoClickButton.BorderSizePixel = 0
ToggleAutoClickButton.Parent = MainFrame
updateToggleButton(ToggleAutoClickButton, _G.AutoClick)

ToggleAutoClickButton.MouseButton1Click:Connect(function()
    _G.AutoClick = not _G.AutoClick
    updateToggleButton(ToggleAutoClickButton, _G.AutoClick)
    print("Auto Click agora está: " .. tostring(_G.AutoClick))
end)

-- Slider para ajuste de Delay do Auto Click
local DelaySliderLabel = Instance.new("TextLabel")
DelaySliderLabel.Name = "DelaySliderLabel"
DelaySliderLabel.Size = UDim2.new(0.9, 0, 0, 15)
DelaySliderLabel.Position = UDim2.new(0.05, 0, 0, ToggleAutoClickButton.Position.Y.Offset + ToggleAutoClickButton.Size.Y.Offset + 5)
DelaySliderLabel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
DelaySliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
DelaySliderLabel.Font = Enum.Font.SourceSans
DelaySliderLabel.TextSize = 12
DelaySliderLabel.TextXAlignment = Enum.TextXAlignment.Left
DelaySliderLabel.Text = "Delay: " .. string.format("%.2f", _G.AutoClickDelay) .. "s (Min: 0.01)"
DelaySliderLabel.Parent = MainFrame

local DelaySlider = Instance.new("Frame")
DelaySlider.Name = "DelaySlider"
DelaySlider.Size = UDim2.new(0.9, 0, 0, 15)
DelaySlider.Position = UDim2.new(0.05, 0, 0, DelaySliderLabel.Position.Y.Offset + DelaySliderLabel.Size.Y.Offset)
DelaySlider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
DelaySlider.BorderSizePixel = 0
DelaySlider.Parent = MainFrame

local SliderButton = Instance.new("TextButton")
SliderButton.Name = "SliderButton"
SliderButton.Size = UDim2.new(0, 20, 1, 0)
SliderButton.Position = UDim2.new(0, 0, 0, 0)
SliderButton.BackgroundColor3 = Color3.fromRGB(100, 100, 200)
SliderButton.Text = ""
SliderButton.BorderSizePixel = 0
SliderButton.Draggable = true
SliderButton.Parent = DelaySlider

local minDelay = 0.01
local maxDelay = 1.0

SliderButton.Draggable = false

local dragging = false
local initialMousePos = 0
local initialButtonPos = 0

SliderButton.MouseButton1Down:Connect(function(x, y)
    dragging = true
    initialMousePos = x
    initialButtonPos = SliderButton.Position.X.Offset
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local newXOffset = initialButtonPos + (input.Position.X - initialMousePos)
        local minX = 0
        local maxX = DelaySlider.AbsoluteSize.X - SliderButton.AbsoluteSize.X

        newXOffset = math.max(minX, math.min(maxX, newXOffset))

        SliderButton.Position = UDim2.new(0, newXOffset, 0, 0)

        local ratio = newXOffset / maxX
        _G.AutoClickDelay = minDelay + (maxDelay - minDelay) * ratio
        DelaySliderLabel.Text = "Delay: " .. string.format("%.2f", _G.AutoClickDelay) .. "s (Min: 0.01)"
    end
end)

game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)


-- Botão STOP ALL FARM
local StopAllFarmButton = Instance.new("TextButton")
StopAllFarmButton.Name = "Stop All Farm"
StopAllFarmButton.Size = UDim2.new(0.9, 0, 0, 30)
StopAllFarmButton.Position = UDim2.new(0.05, 0, 0, MainFrame.Size.Y.Offset - 35)
StopAllFarmButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
StopAllFarmButton.Text = "STOP ALL FARM"
StopAllFarmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
StopAllFarmButton.Font = Enum.Font.SourceSansBold
StopAllFarmButton.TextSize = 16
StopAllFarmButton.BorderSizePixel = 0
StopAllFarmButton.Parent = MainFrame

StopAllFarmButton.MouseButton1Click:Connect(function()
    print("Stop All Farm clicado! (Nenhuma lógica de parada real neste teste)")
    updateStatusText("Status: Inativo")
end)

-- Botão para Esconder/Mostrar a GUI
local HideShowButton = Instance.new("TextButton")
HideShowButton.Name = "Hide/Show"
HideShowButton.Size = UDim2.new(0.4, 0, 0, 15)
HideShowButton.Position = UDim2.new(0.05, 0, 0, MainFrame.Size.Y.Offset - 15)
HideShowButton.Font = Enum.Font.SourceSans
HideShowButton.TextSize = 12
HideShowButton.TextColor3 = Color3.fromRGB(255, 255, 255)
HideShowButton.BackgroundColor3 = Color3.fromRGB(90, 90, 90)
HideShowButton.Text = "Hide"
HideShowButton.BorderSizePixel = 0
HideShowButton.Parent = MainFrame

HideShowButton.MouseButton1Click:Connect(function()
    if MainFrame.Visible then
        MainFrame.Visible = false
        HideShowButton.Text = "Show"
    else
        MainFrame.Visible = true
        HideShowButton.Text = "Hide"
    end
end)

-- Botão para Sair (Destruir a GUI e parar o script)
local ExitButton = Instance.new("TextButton")
ExitButton.Name = "Exit"
ExitButton.Size = UDim2.new(0.4, 0, 0, 15)
ExitButton.Position = UDim2.new(0.55, 0, 0, MainFrame.Size.Y.Offset - 15)
ExitButton.Font = Enum.Font.SourceSans
ExitButton.TextSize = 12
ExitButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ExitButton.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
ExitButton.Text = "Exit"
ExitButton.BorderSizePixel = 0
ExitButton.Parent = MainFrame

ExitButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
    warn("Script de Auto Farm finalizado e GUI destruída.")
end)

-- Inicializa o status na GUI
updateStatusText("Status: Inativo")

-- Mensagem de inicialização
print("Auto Farm GUI carregada. Verifique se a GUI aparece.")
