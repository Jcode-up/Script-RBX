-- Script básico para Blox Fruits
-- Teleporta para inimigos e ataca automaticamente

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local enemiesFolder = workspace.Enemies -- Pasta onde os inimigos estão no jogo

-- Função para teleportar e atacar
function autoFarm()
    for _, enemy in pairs(enemiesFolder:GetChildren()) do
        if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
            -- Teleporta para o inimigo
            character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0)
            wait(0.2)

            -- Ataca o inimigo
            repeat
                wait(0.1)
                enemy.Humanoid.Health = enemy.Humanoid.Health - 10 -- Reduz a vida do inimigo
            until enemy.Humanoid.Health <= 0
        end
    end
end

-- Loop contínuo para farmar automaticamente
while wait(1) do
    autoFarm()
end
