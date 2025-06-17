_G.AutoFarm = true
_G.Weapon = "Combat" -- altere para o nome da arma que deseja usar

function EquipWeapon(weapon)
    if game.Players.LocalPlayer.Backpack:FindFirstChild(weapon) then
        local tool = game.Players.LocalPlayer.Backpack:FindFirstChild(weapon)
        wait(0.3)
        game.Players.LocalPlayer.Character.Humanoid:EquipTool(tool)
    end
end

function GetEnemy()
    for _, v in pairs(game.Workspace.Enemies:GetChildren()) do
        if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
            return v
        end
    end
end

function Attack()
    local Combat = require(game.Players.LocalPlayer.PlayerScripts.CombatFramework)
    local Controller = Combat.activeController
    if Controller and Controller.equipped then
        Controller:attack()
    end
end

spawn(function()
    while _G.AutoFarm do
        local enemy = GetEnemy()
        if enemy then
            EquipWeapon(_G.Weapon)
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame * CFrame.new(0, 10, 5)
            Attack()
        end
        wait()
    end
end)
