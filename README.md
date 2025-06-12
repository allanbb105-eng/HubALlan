-- ✅ CONFIGURAÇÕES GLOBAIS NECESSÁRIAS
_G.AutoFarm = true
IslandESP = true
ESPPlayer = true
ChestESP = true
DevilFruitESP = true
FlowerESP = true
RealFruitESP = true
_G.AutoHop = false -- 🔁 Desativa Hop automático ao entrar (altere para true se quiser)

-- ✅ ESPERA O JOGO CARREGAR E EXECUTA AS FUNÇÕES PRINCIPAIS
repeat wait() until game:IsLoaded()
repeat wait() until game.Players.LocalPlayer
repeat wait() until game.Players.LocalPlayer.Character

-- ✅ EXECUTA FUNÇÕES APÓS TUDO ESTAR PRONTO
task.delay(3, function()
    if CheckQuest then pcall(CheckQuest) end
    if _G.AutoHop and Hop then pcall(Hop) end
end)


function Hop()
    if not _G.AutoHop then return end -- ✅ Impede execução se AutoHop estiver falso

    local PlaceID = game.PlaceId
    local AllIDs = {}
    local foundAnything = ""
    local actualHour = os.date("!*t").hour
    local Deleted = false

    function TPReturner()
        local Site;
        if foundAnything == "" then
            Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100'))
        else
            Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100&cursor=' .. foundAnything))
        end
        local ID = ""
        if Site.nextPageCursor and Site.nextPageCursor ~= "null" and Site.nextPageCursor ~= nil then
            foundAnything = Site.nextPageCursor
        end
        local num = 0;
        for i,v in pairs(Site.data) do
            local Possible = true
            ID = tostring(v.id)
            if tonumber(v.maxPlayers) > tonumber(v.playing) then
                for _,Existing in pairs(AllIDs) do
                    if num ~= 0 then
                        if ID == tostring(Existing) then
                            Possible = false
                        end
                    else
                        if tonumber(actualHour) ~= tonumber(Existing) then
                            pcall(function()
                                AllIDs = {}
                                table.insert(AllIDs, actualHour)
                            end)
                        end
                    end
                    num = num + 1
                end
                if Possible == true then
                    table.insert(AllIDs, ID)
                    wait()
                    pcall(function()
                        wait()
                        game:GetService("TeleportService"):TeleportToPlaceInstance(PlaceID, ID, game.Players.LocalPlayer)
                    end)
                    wait(4)
                end
            end
        end
    end

    function Teleport()
        while wait() do
            if not _G.AutoHop then break end -- ✅ Cancela loop se AutoHop for falso
            pcall(function()
                TPReturner()
                if foundAnything ~= "" then
                    TPReturner()
                end
            end)
        end
    end

    Teleport()
end


-- RESTANTE DO SCRIPT ORIGINAL --

        local ID = ""
        if Site.nextPageCursor and Site.nextPageCursor ~= "null" and Site.nextPageCursor ~= nil then
            foundAnything = Site.nextPageCursor
        
