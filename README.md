-- Configuration Table
-- This table holds all the customizable settings for the script.
-- It's placed at the top for easy access and modification.
local Config = {
    -- Game related configurations
    Game = {
        PlaceId = {
            World1 = 2753915549,
            World2 = 4442272183,
            World3 = 7449423635
        }
    },

    -- Player specific configurations
    Player = {
        KickUnsupportedPlace = true, -- Kick player if the place isn't supported
        RemoveEffect = true,        -- Enable/disable effect removal
        AutoFarmEnabled = true      -- New: Toggle for auto-farming
    },

    -- Developer/Admin Usernames (for auto-hop)
    DevUsernames = {
        "red_game43", "rip_indra", "Axiore", "Polkster", "wenlocktoad",
        "Daigrock", "toilamvidamme", "oofficialnoobie", "Uzoth", "Azarth",
        "arlthmetic", "Death_King", "Lunoven", "TheGreateAced", "rip_fud",
        "drip_mama", "layandikit12", "Hingoi"
    },

    -- Quest data for each world
    -- IMPORTANT: You need to fill this with comprehensive quest data!
    Quests = {
        -- World 1 Quests
        [2753915549] = { -- PlaceId for World 1
            [1] = {Mon = "Bandit", LevelQuest = 1, NameQuest = "BanditQuest1", NameMon = "Bandit", CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544), CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125)},
            [10] = {Mon = "Gorilla", LevelQuest = 10, NameQuest = "GorillaQuest1", NameMon = "Gorilla", CFrameQuest = CFrame.new(1185.04565, 33.0298653, 1420.59607, 0.999999762, -0, -0.0006939987, 0, 1, -0, 0.0006939987, 0, 0.999999762), CFrameMon = CFrame.new(1188.082763671875, 41.53630065917969, 1419.6644287109375)},
            -- Add more World 1 quests here:
            -- [20] = {Mon = "Monkey", LevelQuest = 20, NameQuest = "MonkeyQuest", NameMon = "Monkey", CFrameQuest = CFrame.new(...), CFrameMon = CFrame.new(...)},
            -- [30] = {Mon = "Pirate", LevelQuest = 30, NameQuest = "PirateQuest", NameMon = "Pirate", CFrameQuest = CFrame.new(...), CFrameMon = CFrame.new(...)},
            -- [60] = {Mon = "Brute", LevelQuest = 60, NameQuest = "BruteQuest", NameMon = "Brute", CFrameQuest = CFrame.new(...), CFrameMon = CFrame.new(...)},
            -- ...and so on for all quest levels
        },
        -- World 2 Quests (example structure - YOU NEED TO ADD YOUR DATA)
        [4442272183] = { -- PlaceId for World 2
            [700] = {Mon = "Desert Bandit", LevelQuest = 700, NameQuest = "DesertBanditQuest1", NameMon = "Desert Bandit", CFrameQuest = CFrame.new(100, 50, 100), CFrameMon = CFrame.new(110, 50, 110)},
            -- Add more World 2 quests here
        },
        -- World 3 Quests (example structure - YOU NEED TO ADD YOUR DATA)
        [7449423635] = { -- PlaceId for World 3
            [1500] = {Mon = "Marine Captain", LevelQuest = 1500, NameQuest = "MarineCaptainQuest1", NameMon = "Marine Captain", CFrameQuest = CFrame.new(200, 100, 200), CFrameMon = CFrame.new(210, 100, 210)},
            -- Add more World 3 quests here
        }
    }
}

-- Global Variables (using services for better practice)
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local TeleportService = game:GetService("TeleportService") -- Added for Hop()
local Workspace = game:GetService("Workspace") -- Added for auto-farm
local HttpService = game:GetService("HttpService") -- Added for auto-farm to potentially get current quest from server

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local RootPart = Character:WaitForChild("HumanoidRootPart")

local currentWorldId = game.PlaceId
local currentLevel = 0
local currentQuest = {} -- Will store the current quest details

-- Function to handle unsupported game places
local function handleUnsupportedPlace()
    if Config.Player.KickUnsupportedPlace then
        LocalPlayer:Kick("Game place not supported. Please wait for an update.")
    end
end

-- Function to get the current world based on PlaceId
local function getCurrentWorld()
    if currentWorldId == Config.Game.PlaceId.World1 then
        return 1
    elseif currentWorldId == Config.Game.PlaceId.World2 then
        return 2
    elseif currentWorldId == Config.Game.PlaceId.World3 then
        return 3
    else
        return nil -- Unsupported world
    end
end

-- Function to check and update the current quest based on player level
local function checkAndUpdateQuest()
    currentLevel = LocalPlayer.Data.Level.Value
    local worldQuests = Config.Quests[currentWorldId]

    if not worldQuests then
        print("No quest data configured for the current world: " .. currentWorldId)
        currentQuest = {}
        return
    end

    -- Iterate through quests in reverse order of level to find the highest applicable quest
    local foundQuest = nil
    local sortedLevels = {}
    for level, _ in pairs(worldQuests) do
        table.insert(sortedLevels, level)
    end
    table.sort(sortedLevels, function(a, b) return a > b end) -- Sort descending

    for _, level in ipairs(sortedLevels) do
        if currentLevel >= level then
            foundQuest = worldQuests[level]
            break
        end
    end

    if foundQuest and foundQuest.NameQuest ~= currentQuest.NameQuest then -- Only update if quest changed
        currentQuest = foundQuest
        print("Updated Current Quest: " .. currentQuest.NameQuest .. " (Level: " .. currentQuest.LevelQuest .. ")")
        print("Target Monster: " .. currentQuest.Mon)
    elseif not foundQuest and next(currentQuest) then -- If no quest found but there was one previously
        currentQuest = {}
        print("No quest found for current level: " .. currentLevel)
    end
end

-- Function to handle auto-hopping if a developer is in the server
local function handleAutoHop()
    for _, player in ipairs(Players:GetPlayers()) do
        if table.find(Config.DevUsernames, player.Name) then
            print("Developer detected (" .. player.Name .. "). Auto-hopping...")
            -- Implement the Hop() function here using TeleportService
            TeleportService:Teleport(game.PlaceId) -- Teleport to the same game, which effectively hops servers
            break -- Only need to hop once
        end
    end
end

-- Function to remove specific visual effects (e.g., "Death" effects)
local function removeEffects()
    if Config.Player.RemoveEffect then
        local effectContainer = ReplicatedStorage:FindFirstChild("Effect") and ReplicatedStorage.Effect:FindFirstChild("Container")
        if effectContainer then
            for _, effect in ipairs(effectContainer:GetChildren()) do
                if effect.Name == "Death" then
                    effect:Destroy()
                end
            end
        end
    end
end

-- Basic Auto-Farm Function (Simplified for demonstration)
local function autoFarm()
    if not Config.Player.AutoFarmEnabled or not currentQuest or not currentQuest.CFrameMon then
        return
    end

    local targetMonsterPos = currentQuest.CFrameMon.p
    local playerPos = RootPart.CFrame.p

    -- Teleport to the monster's CFrame location
    if (playerPos - targetMonsterPos).Magnitude > 5 then -- If far from target
        RootPart.CFrame = CFrame.new(targetMonsterPos)
        print("Teleporting to quest monster: " .. currentQuest.NameMon)
        task.wait(0.5) -- Wait a bit after teleporting
    end

    -- Find the target monster
    local targetMonster = nil
    for _, v in ipairs(Workspace:GetChildren()) do
        if v:IsA("Model") and v.Name:find(currentQuest.NameMon, 1, true) and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
            targetMonster = v
            break
        end
    end

    if targetMonster then
        -- Attack the monster (this is highly simplified and depends on your specific executor's capabilities)
        -- A common way to attack in exploits is to set the Humanoid's target or fire remote events.
        -- This is a placeholder for actual attack logic.
        Humanoid:ChangeState(Enum.HumanoidStateType.Running) -- Ensure player is in a movable state
        Humanoid:SetAttribute("Target", targetMonster) -- Some exploits use this for auto-attack

        -- If the game has a combat remote, you might try to fire it. This is highly game-specific.
        -- Example (hypothetical):
        -- ReplicatedStorage.CombatEvent:FireServer(targetMonster, "M1_Attack")

        print("Attacking: " .. targetMonster.Name .. " at " .. tostring(targetMonster.HumanoidRootPart.Position))
        task.wait(0.1) -- Short wait for attack
    else
        print("Target monster not found or already defeated: " .. currentQuest.NameMon)
        -- Potentially move to the quest giver to accept/complete quest if no monsters are found
        -- RootPart.CFrame = currentQuest.CFrameQuest -- Teleport to quest giver
        task.wait(1) -- Wait before re-checking for monsters
    end
end


-- Main script execution starts here
local currentWorld = getCurrentWorld()
if not currentWorld then
    handleUnsupportedPlace()
else
    print("Welcome to Blox Fruits World " .. currentWorld .. "!")

    -- Initial quest check
    checkAndUpdateQuest()

    -- Connect to level change for dynamic quest updates
    LocalPlayer.Data.Level.Changed:Connect(function()
        checkAndUpdateQuest()
    end)

    -- Main loop for automation tasks
    spawn(function()
        while task.wait(0.1) do -- Use a frequent but not overly aggressive wait time
            handleAutoHop()
            removeEffects()
            autoFarm() -- Call the auto-farm function
        end
    end)
end

print("Blox Fruits Automation Script Loaded!")
