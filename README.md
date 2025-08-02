local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Coordenadas personalizadas (usando CFrame corretamente)
local teleportPoints = {
    CFrame.new(-1463.15393, 22.4496212, 5672.29102), -- Ponto 1
    CFrame.new(-2022.13855, 30.5247879, 5564.81445)  -- Ponto 2
}

-- Referências à interface
local gui = script.Parent
local label = gui:WaitForChild("TeleportLabel")
local button = gui:WaitForChild("ToggleButton")

local currentIndex = 1
local teleportEnabled = false

-- Função segura para pegar o HumanoidRootPart
local function getHumanoidRootPart()
    local character = player.Character or player.CharacterAdded:Wait()
    local hrp = character:FindFirstChild("HumanoidRootPart")

    local timeout = 5  -- segundos
    local timer = 0

    while not hrp and timer < timeout do
        wait(0.1)
        timer += 0.1
        character = player.Character or player.CharacterAdded:Wait()
        hrp = character:FindFirstChild("HumanoidRootPart")
    end

    return hrp
end

-- Exibir mensagem temporária de teleporte
local function showTeleportMessage(index)
    label.Text = "Teleporte: Lugar " .. index
    label.Visible = true
    wait(2.5)
    label.Visible = false
end

-- Quando o botão for clicado
button.MouseButton1Click:Connect(function()
    -- Alterna entre ponto 1 e 2
    currentIndex = currentIndex == 1 and 2 or 1

    -- Tenta obter o HumanoidRootPart
    local humanoidRootPart = getHumanoidRootPart()
    if humanoidRootPart then
        humanoidRootPart.CFrame = teleportPoints[currentIndex]
        showTeleportMessage(currentIndex)
    else
        warn("Não foi possível encontrar o HumanoidRootPart do personagem.")
    end
end)
