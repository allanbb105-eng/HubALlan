-- Variáveis globais de controle
World1, World2, World3 = false, false, false
IslandESP = false
ESPPlayer = false
ChestESP = false
DevilFruitESP = false
FlowerESP = false
RealFruitESP = false
_G.AutoFarm = false

-- Verifica o mundo atual
if game.PlaceId == 2753915549 then
    World1 = true
elseif game.PlaceId == 4442272183 then
    World2 = true
elseif game.PlaceId == 7449423635 then
    World3 = true
else
    game:GetService("Players").LocalPlayer:Kick("Do not Support, Please wait...")
end

-- Função para arredondar
local function round(n)
    return math.floor(tonumber(n) + 0.5)
end

-- Função para verificar se valor é nulo
function isnil(thing)
    return (thing == nil)
end

-- Exemplo de correção para CheckQuest com comparações corretas
function CheckQuest()
    local MyLevel = game:GetService("Players").LocalPlayer.Data.Level.Value
    if World1 then
        if MyLevel >= 1 and MyLevel <= 9 then
            Mon = "Bandit"
            LevelQuest = 1
            NameQuest = "BanditQuest1"
            NameMon = "Bandit"
            CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231)
            CFrameMon = CFrame.new(1045.9626, 27.0025, 1560.8203)
        elseif MyLevel >= 10 and MyLevel <= 14 then
            Mon = "Monkey"
            LevelQuest = 1
            NameQuest = "JungleQuest"
            NameMon = "Monkey"
            CFrameQuest = CFrame.new(-1598.0891, 35.5501, 153.3778)
            CFrameMon = CFrame.new(-1448.5180, 67.8530, 11.4658)
        end
        -- (Continue com os demais níveis da mesma forma)
    end
end

-- Corrigido: uso correto de TextSize e Enum.Font
function CreateBillboard(parent, text, color)
    local bill = Instance.new("BillboardGui", parent)
    bill.Name = "NameEsp"
    bill.ExtentsOffset = Vector3.new(0, 1, 0)
    bill.Size = UDim2.new(1, 200, 1, 30)
    bill.Adornee = parent
    bill.AlwaysOnTop = true

    local name = Instance.new("TextLabel", bill)
    name.Font = Enum.Font.GothamBold
    name.TextSize = 14
    name.TextWrapped = true
    name.Size = UDim2.new(1, 0, 1, 0)
    name.TextYAlignment = Enum.TextYAlignment.Top
    name.BackgroundTransparency = 1
    name.TextStrokeTransparency = 0.5
    name.TextColor3 = color
    name.Text = text
end

-- Exemplo de ESP básico corrigido
function UpdatePlayerChams()
    for _, v in pairs(game:GetService("Players"):GetChildren()) do
        pcall(function()
            if v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                local head = v.Character.Head
                if ESPPlayer and not head:FindFirstChild("NameEsp") then
                    CreateBillboard(head, v.Name, Color3.fromRGB(255, 0, 0))
                elseif not ESPPlayer and head:FindFirstChild("NameEsp") then
                    head.NameEsp:Destroy()
                end
            end
        end)
    end
end

-- Adicione outras funções aqui conforme as correções
-- Certifique-se de aplicar comparações com >= e <= para os níveis
-- Substitua todos os FontSize incorretos por TextSize
-- Valide objetos com FindFirstChild antes de acessar

-- O resto das funções (Hop, ESPs, etc.) devem seguir a mesma lógica de segurança e correção
-- Se quiser que eu reescreva tudo, posso continuar aqui.
