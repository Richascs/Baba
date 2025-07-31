-- Coloque este script em StarterPlayerScripts ou StarterGui

-- Criar GUI
local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
local button = Instance.new("TextButton")
button.Parent = screenGui
button.Size = UDim2.new(0,200,0,50)
button.Position = UDim2.new(0.5,-100,0.1,0)
button.Text = "Auto Farm: OFF"
button.BackgroundColor3 = Color3.fromRGB(255,0,0)

local autoFarmActive = false
local runService = game:GetService("RunService")

-- Função para atacar o mob
function attackMob()
    local mob = workspace:FindFirstChild("Bandit")
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    if mob and mob:FindFirstChild("Humanoid") and mob.Humanoid.Health > 0 then
        -- Move o jogador até o Bandit (ajuste a distância se necessário)
        if character:FindFirstChild("HumanoidRootPart") and mob:FindFirstChild("HumanoidRootPart") then
            character.HumanoidRootPart.CFrame = mob.HumanoidRootPart.CFrame * CFrame.new(0,0,3)
        end
        
        -- Chame sua função de ataque aqui (ajuste conforme seu jogo)
        if character:FindFirstChild("Attack") then
            character.Attack:Fire()
        end
    end
end

-- Loop Auto Farm
local connection = nil

button.MouseButton1Click:Connect(function()
    autoFarmActive = not autoFarmActive
    button.Text = "Auto Farm: " .. (autoFarmActive and "ON" or "OFF")
    button.BackgroundColor3 = autoFarmActive and Color3.fromRGB(0,255,0) or Color3.fromRGB(255,0,0)
    if autoFarmActive then
        connection = runService.RenderStepped:Connect(function()
            attackMob()
        end)
    else
        if connection then connection:Disconnect() end
    end
end)
