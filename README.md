-- LocalScript dentro de TeleportGui
local player = game.Players.LocalPlayer
local button = script.Parent:WaitForChild("ToggleButton")

-- Coordenadas de teleporte (substitua pelas suas)
local positionA = Vector3.new(-2022.13855, 30.5247879, 5564.81445, 1, 0, 0, 0, 1, 0, 0, 0, 1)
local positionB = Vector3.new(-1463.15393, 22.4496212, 5672.29102, 1, 0, 0, 0, 1, 0, 0, 0, 1)

-- Estado do teleporte
local teleportActive = false

-- Função de teleporte automático
local function startTeleportLoop()
    spawn(function()
        while teleportActive do
            local character = player.Character or player.CharacterAdded:Wait()
            local root = character:WaitForChild("HumanoidRootPart")

            root.CFrame = CFrame.new(positionA)
            wait(2)
            root.CFrame = CFrame.new(positionB)
            wait(2)
        end
    end)
end

-- Clique no botão
button.MouseButton1Click:Connect(function()
    teleportActive = not teleportActive

    if teleportActive then
        button.Text = "Desativar Farm"
        startTeleportLoop()
    else
        button.Text = "Ativar Farm"
    end
end)
