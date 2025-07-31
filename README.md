-- Script de Auto Farm para atacar Snow Bandit no Blox Fruits
-- Apenas para fins educacionais/teste de segurança

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

function findSnowBandit()
    for _, v in pairs(workspace.Enemies:GetChildren()) do
        if v.Name == "Snow Bandit" and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
            return v
        end
    end
    return nil
end

function equipWeapon(Blackleg)
    for _, v in pairs(LocalPlayer.Backpack:GetChildren()) do
        if v.Name == weaponName then
            LocalPlayer.Character.Humanoid:EquipTool(v)
            break
        end
    end
end

local weapon = "Katana" -- Mude para a arma desejada, precisa estar no seu inventário

while true do
    local npc = findSnowBandit()
    if npc then
        equipWeapon(weapon)
        LocalPlayer.Character.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame + Vector3.new(0,3,0)
        -- Ataca o NPC
        for i = 1, 3 do
            game:GetService("ReplicatedStorage").Remotes.Combat:FireServer(npc)
            wait(0.2)
        end
    end
    wait(1)
end
