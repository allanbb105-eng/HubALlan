-- Define variáveis de serviço para acesso mais fácil
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportService = game:GetService("TeleportService")
local RunService = game:GetService("RunService")

-- Adiciona "Allan Hub" na interface, conforme solicitado anteriormente.
-- Isso cria uma ScreenGui e um TextLabel para exibir o nome do script.
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AllanHubScriptUI"
ScreenGui.Parent = PlayerGui

local TextLabel = Instance.new("TextLabel")
TextLabel.Size = UDim2.new(0.2, 0, 0.05, 0)
TextLabel.Position = UDim2.new(0.01, 0, 0.01, 0)
TextLabel.BackgroundTransparency = 1
TextLabel.TextColor3 = Color3.new(1, 1, 1)
TextLabel.Font = Enum.Font.SourceSansBold
TextLabel.TextSize = 24
TextLabel.Text = "Allan Hub"
TextLabel.Parent = ScreenGui


-- Variáveis globais (usadas com _G para serem acessíveis de qualquer lugar no script)
_G.AutoFarm = false -- Controla se o auto-farm está ativo
_G.Mob = nil        -- Nome do mob alvo para o farm
_G.AutoQuest = false -- Controla se o auto-quest está ativo
_G.Teleport = false -- Controla se o teleporte está ativo
_G.KillAura = false -- Controla se o Kill Aura está ativo
_G.AntiLag = false  -- Controla se o anti-lag está ativo
_G.Remove_Effect = true -- Controla se os efeitos visuais são removidos

-- Variáveis para identificar o mundo atual com base no PlaceId (ID do jogo/servidor)
local World1 = false
local World2 = false
local World3 = false

-- Verifica o PlaceId do jogo e define a variável de mundo correspondente
if game.PlaceId == 2753915549 then
    World1 = true
elseif game.PlaceId == 4442272183 then
    World2 = true
elseif game.PlaceId == 7449423635 then
    World3 = true
else
    -- Se o PlaceId não for reconhecido, o jogador é kickado do servidor
    game:GetService("Players").LocalPlayer:Kick("Do not Support, Please wait...")
end

-- Função para verificar a quest atual e definir o mob alvo e localização
function CheckQuest()
    MyLevel = LocalPlayer.Data.Level.Value -- Pega o nível do jogador

    if World1 then
        -- Lógica de quests para o Mundo 1
        if MyLevel >= 1 and MyLevel <= 9 then
            Mon = "Bandit"
            LevelQuest = 1
            NameQuest = "BanditQuest1"
            NameMon = "Bandit"
            CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544)
            CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125)
        elseif MyLevel >= 10 and MyLevel <= 29 then
            Mon = "Gorilla"
            LevelQuest = 10
            NameQuest = "GorillaQuest1"
            NameMon = "Gorilla"
            CFrameQuest = CFrame.new(1005.47467, 15.4495068, 1713.88562, 0.866025448, -0, -0.5, 0, 1, -0, 0.5, 0, 0.866025448)
            CFrameMon = CFrame.new(1001.0772094726562, 27.00250816345215, 1729.8055419921875)
        elseif MyLevel >= 30 and MyLevel <= 59 then
            Mon = "Boss_Gorilla"
            LevelQuest = 30
            NameQuest = "Boss_GorillaQuest"
            NameMon = "Boss_Gorilla"
            CFrameQuest = CFrame.new(1005.47467, 15.4495068, 1713.88562, 0.866025448, -0, -0.5, 0, 1, -0, 0.5, 0, 0.866025448)
            CFrameMon = CFrame.new(1001.0772094726562, 27.00250816345215, 1729.8055419921875)
        elseif MyLevel >= 60 and MyLevel <= 89 then
            Mon = "Pirate"
            LevelQuest = 60
            NameQuest = "PirateQuest"
            NameMon = "Pirate"
            CFrameQuest = CFrame.new(1251.3468, 15.4495068, 1761.64294, -0.0174524061, -0, 0.999847651, 0, 1, -0, -0.999847651, 0, -0.0174524061)
            CFrameMon = CFrame.new(1251.5284423828125, 27.00250816345215, 1745.385009765625)
        elseif MyLevel >= 90 and MyLevel <= 99 then
            Mon = "Pirate_Captain"
            LevelQuest = 90
            NameQuest = "Pirate_CaptainQuest"
            NameMon = "Pirate_Captain"
            CFrameQuest = CFrame.new(1251.3468, 15.4495068, 1761.64294, -0.0174524061, -0, 0.999847651, 0, 1, -0, -0.999847651, 0, -0.0174524061)
            CFrameMon = CFrame.new(1251.5284423828125, 27.00250816345215, 1745.385009765625)
        elseif MyLevel >= 100 and MyLevel <= 119 then
            Mon = "Brute"
            LevelQuest = 100
            NameQuest = "BruteQuest"
            NameMon = "Brute"
            CFrameQuest = CFrame.new(1542.45715, 15.4495068, 1698.81311, -0.829037547, -0, 0.559201956, 0, 1, -0, -0.559201956, 0, -0.829037547)
            CFrameMon = CFrame.new(1553.86474609375, 27.00250816345215, 1696.082763671875)
        elseif MyLevel >= 120 and MyLevel <= 149 then
            Mon = "Chief_Brute"
            LevelQuest = 120
            NameQuest = "Chief_BruteQuest"
            NameMon = "Chief_Brute"
            CFrameQuest = CFrame.new(1542.45715, 15.4495068, 1698.81311, -0.829037547, -0, 0.559201956, 0, 1, -0, -0.559201956, 0, -0.829037547)
            CFrameMon = CFrame.new(1553.86474609375, 27.00250816345215, 1696.082763671875)
        elseif MyLevel >= 150 and MyLevel <= 179 then
            Mon = "Shark"
            LevelQuest = 150
            NameQuest = "SharkQuest"
            NameMon = "Shark"
            CFrameQuest = CFrame.new(1470.93286, 1.44950676, 1445.89539, -0.965925872, -0, 0.258819073, 0, 1, -0, -0.258819073, 0, -0.965925872)
            CFrameMon = CFrame.new(1476.3262939453125, 27.00250816345215, 1450.4132080078125)
        elseif MyLevel >= 180 and MyLevel <= 199 then
            Mon = "Saw"
            LevelQuest = 180
            NameQuest = "SawQuest"
            NameMon = "Saw"
            CFrameQuest = CFrame.new(1470.93286, 1.44950676, 1445.89539, -0.965925872, -0, 0.258819073, 0, 1, -0, -0.258819073, 0, -0.965925872)
            CFrameMon = CFrame.new(1476.3262939453125, 27.00250816345215, 1450.4132080078125)
        elseif MyLevel >= 200 and MyLevel <= 249 then
            Mon = "Fishman"
            LevelQuest = 200
            NameQuest = "FishmanQuest"
            NameMon = "Fishman"
            CFrameQuest = CFrame.new(1209.68066, 15.4495068, 1269.04016, -0.939692676, -0, 0.342020124, 0, 1, -0, -0.342020124, 0, -0.939692676)
            CFrameMon = CFrame.new(1216.73291015625, 27.00250816345215, 1269.9576416015625)
        elseif MyLevel >= 250 and MyLevel <= 299 then
            Mon = "Fishman_Lord"
            LevelQuest = 250
            NameQuest = "Fishman_LordQuest"
            NameMon = "Fishman_Lord"
            CFrameQuest = CFrame.new(1209.68066, 15.4495068, 1269.04016, -0.939692676, -0, 0.342020124, 0, 1, -0, -0.342020124, 0, -0.939692676)
            CFrameMon = CFrame.new(1216.73291015625, 27.00250816345215, 1269.9576416015625)
        elseif MyLevel >= 300 and MyLevel <= 349 then
            Mon = "Magma"
            LevelQuest = 300
            NameQuest = "MagmaQuest"
            NameMon = "Magma"
            CFrameQuest = CFrame.new(1039.23193, 15.4495068, 1140.23157, -0.681998491, -0, 0.731402934, 0, 1, -0, -0.731402934, 0, -0.681998491)
            CFrameMon = CFrame.new(1045.228515625, 27.00250816345215, 1140.912841796875)
        elseif MyLevel >= 350 and MyLevel <= 399 then
            Mon = "Magma_Admiral"
            LevelQuest = 350
            NameQuest = "Magma_AdmiralQuest"
            NameMon = "Magma_Admiral"
            CFrameQuest = CFrame.new(1039.23193, 15.4495068, 1140.23157, -0.681998491, -0, 0.731402934, 0, 1, -0, -0.731402934, 0, -0.681998491)
            CFrameMon = CFrame.new(1045.228515625, 27.00250816345215, 1140.912841796875)
        elseif MyLevel >= 400 and MyLevel <= 449 then
            Mon = "Warden"
            LevelQuest = 400
            NameQuest = "WardenQuest"
            NameMon = "Warden"
            CFrameQuest = CFrame.new(1099.98755, 15.4495068, 836.786987, -0.999939024, -0, 0.0109951682, 0, 1, -0, -0.0109951682, 0, -0.999939024)
            CFrameMon = CFrame.new(1106.9423828125, 27.00250816345215, 836.7214965820312)
        elseif MyLevel >= 450 and MyLevel <= 499 then
            Mon = "Chief_Warden"
            LevelQuest = 450
            NameQuest = "Chief_WardenQuest"
            NameMon = "Chief_Warden"
            CFrameQuest = CFrame.new(1099.98755, 15.4495068, 836.786987, -0.999939024, -0, 0.0109951682, 0, 1, -0, -0.0109951682, 0, -0.999939024)
            CFrameMon = CFrame.new(1106.9423828125, 27.00250816345215, 836.7214965820312)
        elseif MyLevel >= 500 and MyLevel <= 549 then
            Mon = "Marine"
            LevelQuest = 500
            NameQuest = "MarineQuest"
            NameMon = "Marine"
            CFrameQuest = CFrame.new(1473.42932, 15.4495068, 878.503906, -0.99862951, -0, -0.0523359627, 0, 1, -0, 0.0523359627, 0, -0.99862951)
            CFrameMon = CFrame.new(1473.197998046875, 27.00250816345215, 895.748779296875)
        elseif MyLevel >= 550 and MyLevel <= 599 then
            Mon = "Vice_Admiral"
            LevelQuest = 550
            NameQuest = "Vice_AdmiralQuest"
            NameMon = "Vice_Admiral"
            CFrameQuest = CFrame.new(1473.42932, 15.4495068, 878.503906, -0.99862951, -0, -0.0523359627, 0, 1, -0, 0.0523359627, 0, -0.99862951)
            CFrameMon = CFrame.new(1473.197998046875, 27.00250816345215, 895.748779296875)
        elseif MyLevel >= 600 and MyLevel <= 649 then
            Mon = "Impel_Down"
            LevelQuest = 600
            NameQuest = "Impel_DownQuest"
            NameMon = "Impel_Down"
            CFrameQuest = CFrame.new(1473.42932, 15.4495068, 878.503906, -0.99862951, -0, -0.0523359627, 0, 1, -0, 0.0523359627, 0, -0.99862951)
            CFrameMon = CFrame.new(1473.197998046875, 27.00250816345215, 895.748779296875)
        elseif MyLevel >= 650 and MyLevel <= 699 then
            Mon = "Warlord"
            LevelQuest = 650
            NameQuest = "WarlordQuest"
            NameMon = "Warlord"
            CFrameQuest = CFrame.new(1473.42932, 15.4495068, 878.503906, -0.99862951, -0, -0.0523359627, 0, 1, -0, 0.0523359627, 0, -0.99862951)
            CFrameMon = CFrame.new(1473.197998046875, 27.00250816345215, 895.748779296875)
        elseif MyLevel >= 700 then
            Mon = "Saber"
            LevelQuest = 700
            NameQuest = "SaberQuest"
            NameMon = "Saber"
            CFrameQuest = CFrame.new(1542.45715, 15.4495068, 1698.81311, -0.829037547, -0, 0.559201956, 0, 1, -0, -0.559201956, 0, -0.829037547)
            CFrameMon = CFrame.new(1553.86474609375, 27.00250816345215, 1696.082763671875)
        end
    elseif World2 then
        -- Lógica de quests para o Mundo 2 (similar ao Mundo 1, mas com diferentes mobs e CFrames)
        if MyLevel >= 700 and MyLevel <= 749 then
            Mon = "Dark_Step"
            LevelQuest = 700
            NameQuest = "Dark_StepQuest"
            NameMon = "Dark_Step"
            CFrameQuest = CFrame.new(-1052.17932, 401.768677, 365.110596, -0.874278486, -0, 0.485605806, 0, 1, -0, -0.485605806, 0, -0.874278486)
            CFrameMon = CFrame.new(-1046.037109375, 411.00250244140625, 359.8824157714844)
        elseif MyLevel >= 750 and MyLevel <= 799 then
            Mon = "Snow_Bandit"
            LevelQuest = 750
            NameQuest = "Snow_BanditQuest"
            NameMon = "Snow_Bandit"
            CFrameQuest = CFrame.new(-1009.68958, 401.768677, 437.054321, 0.965925872, -0, -0.258819073, 0, 1, -0, 0.258819073, 0, 0.965925872)
            CFrameMon = CFrame.new(-1009.1309814453125, 411.00250244140625, 421.4390869140625)
        elseif MyLevel >= 800 and MyLevel <= 849 then
            Mon = "Yeti"
            LevelQuest = 800
            NameQuest = "YetiQuest"
            NameMon = "Yeti"
            CFrameQuest = CFrame.new(-1009.68958, 401.768677, 437.054321, 0.965925872, -0, -0.258819073, 0, 1, -0, 0.258819073, 0, 0.965925872)
            CFrameMon = CFrame.new(-1009.1309814453125, 411.00250244140625, 421.4390869140625)
        elseif MyLevel >= 850 and MyLevel <= 899 then
            Mon = "Military"
            LevelQuest = 850
            NameQuest = "MilitaryQuest"
            NameMon = "Military"
            CFrameQuest = CFrame.new(-870.760742, 401.768677, 513.784424, 0.829037547, -0, -0.559201956, 0, 1, -0, 0.559201956, 0, 0.829037547)
            CFrameMon = CFrame.new(-876.549560546875, 411.00250244140625, 520.1983642578125)
        elseif MyLevel >= 900 and MyLevel <= 949 then
            Mon = "Soldier"
            LevelQuest = 900
            NameQuest = "SoldierQuest"
            NameMon = "Soldier"
            CFrameQuest = CFrame.new(-870.760742, 401.768677, 513.784424, 0.829037547, -0, -0.559201956, 0, 1, -0, 0.559201956, 0, 0.829037547)
            CFrameMon = CFrame.new(-876.549560546875, 411.00250244140625, 520.1983642578125)
        elseif MyLevel >= 950 and MyLevel <= 999 then
            Mon = "Franky"
            LevelQuest = 950
            NameQuest = "FrankyQuest"
            NameMon = "Franky"
            CFrameQuest = CFrame.new(-682.028625, 401.768677, 563.856934, 0.999787152, -0, -0.0206351057, 0, 1, -0, 0.0206351057, 0, 0.999787152)
            CFrameMon = CFrame.new(-692.6580810546875, 411.00250244140625, 560.83544921875)
        elseif MyLevel >= 1000 and MyLevel <= 1049 then
            Mon = "Donut_King"
            LevelQuest = 1000
            NameQuest = "Donut_KingQuest"
            NameMon = "Donut_King"
            CFrameQuest = CFrame.new(-682.028625, 401.768677, 563.856934, 0.999787152, -0, -0.0206351057, 0, 1, -0, 0.0206351057, 0, 0.999787152)
            CFrameMon = CFrame.new(-692.6580810546875, 411.00250244140625, 560.83544921875)
        elseif MyLevel >= 1050 and MyLevel <= 1099 then
            Mon = "Longma"
            LevelQuest = 1050
            NameQuest = "LongmaQuest"
            NameMon = "Longma"
            CFrameQuest = CFrame.new(-682.028625, 401.768677, 563.856934, 0.999787152, -0, -0.0206351057, 0, 1, -0, 0.0206351057, 0, 0.999787152)
            CFrameMon = CFrame.new(-692.6580810546875, 411.00250244140625, 560.83544921875)
        elseif MyLevel >= 1100 and MyLevel <= 1149 then
            Mon = "Kilo_Admiral"
            LevelQuest = 1100
            NameQuest = "Kilo_AdmiralQuest"
            NameMon = "Kilo_Admiral"
            CFrameQuest = CFrame.new(-485.496277, 401.768677, 650.046021, 0.999974251, -0, 0.00717469742, 0, 1, -0, -0.00717469742, 0, 0.999974251)
            CFrameMon = CFrame.new(-485.496277, 411.00250244140625, 650.046021)
        elseif MyLevel >= 1150 and MyLevel <= 1199 then
            Mon = "Order"
            LevelQuest = 1150
            NameQuest = "OrderQuest"
            NameMon = "Order"
            CFrameQuest = CFrame.new(-485.496277, 401.768677, 650.046021, 0.999974251, -0, 0.00717469742, 0, 1, -0, -0.00717469742, 0, 0.999974251)
            CFrameMon = CFrame.new(-485.496277, 411.00250244140625, 650.046021)
        elseif MyLevel >= 1200 and MyLevel <= 1249 then
            Mon = "Awakened_Buddha"
            LevelQuest = 1200
            NameQuest = "Awakened_BuddhaQuest"
            NameMon = "Awakened_Buddha"
            CFrameQuest = CFrame.new(-380.089966, 401.768677, 668.618652, 0.939692676, -0, 0.342020124, 0, 1, -0, -0.342020124, 0, 0.939692676)
            CFrameMon = CFrame.new(-370.0899658203125, 411.00250244140625, 672.61865234375)
        elseif MyLevel >= 1250 and MyLevel <= 1299 then
            Mon = "Cake_Prince"
            LevelQuest = 1250
            NameQuest = "Cake_PrinceQuest"
            NameMon = "Cake_Prince"
            CFrameQuest = CFrame.new(-380.089966, 401.768677, 668.618652, 0.939692676, -0, 0.342020124, 0, 1, -0, -0.342020124, 0, 0.939692676)
            CFrameMon = CFrame.new(-370.0899658203125, 411.00250244140625, 672.61865234375)
        elseif MyLevel >= 1300 and MyLevel <= 1349 then
            Mon = "Don_Swan"
            LevelQuest = 1300
            NameQuest = "Don_SwanQuest"
            NameMon = "Don_Swan"
            CFrameQuest = CFrame.new(-1052.17932, 401.768677, 365.110596, -0.874278486, -0, 0.485605806, 0, 1, -0, -0.485605806, 0, -0.874278486)
            CFrameMon = CFrame.new(-1046.037109375, 411.00250244140625, 359.8824157714844)
        elseif MyLevel >= 1350 and MyLevel <= 1399 then
            Mon = "Dark_Beard"
            LevelQuest = 1350
            NameQuest = "Dark_BeardQuest"
            NameMon = "Dark_Beard"
            CFrameQuest = CFrame.new(-1052.17932, 401.768677, 365.110596, -0.874278486, -0, 0.485605806, 0, 1, -0, -0.485605806, 0, -0.874278486)
            CFrameMon = CFrame.new(-1046.037109375, 411.00250244140625, 359.8824157714844)
        elseif MyLevel >= 1400 then
            Mon = "Katukuri"
            LevelQuest = 1400
            NameQuest = "KatukuriQuest"
            NameMon = "Katukuri"
            CFrameQuest = CFrame.new(-1052.17932, 401.768677, 365.110596, -0.874278486, -0, 0.485605806, 0, 1, -0, -0.485605806, 0, -0.874278486)
            CFrameMon = CFrame.new(-1046.037109375, 411.00250244140625, 359.8824157714844)
        end
    elseif World3 then
        -- Lógica de quests para o Mundo 3 (similar aos anteriores, com diferentes mobs e CFrames)
        if MyLevel >= 1500 and MyLevel <= 1549 then
            Mon = "Luffy"
            LevelQuest = 1500
            NameQuest = "LuffyQuest"
            NameMon = "Luffy"
            CFrameQuest = CFrame.new(7700.73047, 509.309021, -3878.07764, -0.587785244, -0, 0.809017003, 0, 1, -0, -0.809017003, 0, -0.587785244)
            CFrameMon = CFrame.new(7704.91259765625, 517.0025024414062, -3884.2255859375)
        elseif MyLevel >= 1550 and MyLevel <= 1599 then
            Mon = "King_Red"
            LevelQuest = 1550
            NameQuest = "King_RedQuest"
            NameMon = "King_Red"
            CFrameQuest = CFrame.new(7700.73047, 509.309021, -3878.07764, -0.587785244, -0, 0.809017003, 0, 1, -0, -0.809017003, 0, -0.587785244)
            CFrameMon = CFrame.new(7704.91259765625, 517.0025024414062, -3884.2255859375)
        elseif MyLevel >= 1600 and MyLevel <= 1649 then
            Mon = "Dragon"
            LevelQuest = 1600
            NameQuest = "DragonQuest"
            NameMon = "Dragon"
            CFrameQuest = CFrame.new(7700.73047, 509.309021, -3878.07764, -0.587785244, -0, 0.809017003, 0, 1, -0, -0.809017003, 0, -0.587785244)
            CFrameMon = CFrame.new(7704.91259765625, 517.0025024414062, -3884.2255859375)
        elseif MyLevel >= 1650 and MyLevel <= 1699 then
            Mon = "God_Usop"
            LevelQuest = 1650
            NameQuest = "God_UsopQuest"
            NameMon = "God_Usop"
            CFrameQuest = CFrame.new(7700.73047, 509.309021, -3878.07764, -0.587785244, -0, 0.809017003, 0, 1, -0, -0.809017003, 0, -0.587785244)
            CFrameMon = CFrame.new(7704.91259765625, 517.0025024414062, -3884.2255859375)
        elseif MyLevel >= 1700 and MyLevel <= 1749 then
            Mon = "Doflamingo"
            LevelQuest = 1700
            NameQuest = "DoflamingoQuest"
            NameMon = "Doflamingo"
            CFrameQuest = CFrame.new(7700.73047, 509.309021, -3878.07764, -0.587785244, -0, 0.809017003, 0, 1, -0, -0.809017003, 0, -0.587785244)
            CFrameMon = CFrame.new(7704.91259765625, 517.0025024414062, -3884.2255859375)
        elseif MyLevel >= 1750 and MyLevel <= 1799 then
            Mon = "Kaido"
            LevelQuest = 1750
            NameQuest = "KaidoQuest"
            NameMon = "Kaido"
            CFrameQuest = CFrame.new(7700.73047, 509.309021, -3878.07764, -0.587785244, -0, 0.809017003, 0, 1, -0, -0.809017003, 0, -0.587785244)
            CFrameMon = CFrame.new(7704.91259765625, 517.0025024414062, -3884.2255859375)
        elseif MyLevel >= 1800 and MyLevel <= 1849 then
            Mon = "Big_Mom"
            LevelQuest = 1800
            NameQuest = "Big_MomQuest"
            NameMon = "Big_Mom"
            CFrameQuest = CFrame.new(7700.73047, 509.309
