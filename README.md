#!/usr/bin/env lua

-- Função para verificar o mundo atual
if game.PlaceId == 2753915549 then
    World1 = true
elseif game.PlaceId == 4442272183 then
    World2 = true
elseif game.PlaceId == 7449423635 then
    World3 = true
else
    game:GetService("Players").LocalPlayer:Kick("Do not Support, Please wait...")
end

-- Função para verificar a missão baseada no nível do jogador
function CheckQuest()
    MyLevel = game:GetService("Players").LocalPlayer.Data.Level.Value
    if World1 then
        if MyLevel == 1 or MyLevel <= 9 then
            Mon = "Bandit"
            LevelQuest = 1
            NameQuest = "BanditQuest1"
            NameMon = "Bandit"
            CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231)
            CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125)
        end
    end
end

-- Interface gráfica usando LÖVE
function love.load()
    font = love.graphics.newFont(14)
    love.graphics.setFont(font)
    button = {x = 100, y = 150, width = 200, height = 50, text = "Clique Aqui!"}
end

function love.draw()
    love.graphics.print("Bem-vindo ao Script!", 100, 100)

    -- Desenha o botão
    love.graphics.rectangle("fill", button.x, button.y, button.width, button.height)
    love.graphics.setColor(1, 1, 1) -- Cor do texto
    love.graphics.print(button.text, button.x + 50, button.y + 15)
    love.graphics.setColor(1, 1, 1) -- Restaura a cor original
end

function love.mousepressed(x, y, button_pressed)
    if button_pressed == 1 and x >= button.x and x <= button.x + button.width and y >= button.y and y <= button.y + button.height then
        print("Botão pressionado! Executando ação...")
    end
end
