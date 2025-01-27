-- Configuração inicial do script
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local workspace = game.Workspace
local httpService = game:GetService("HttpService")

-- Criando o menu GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 150)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.5
frame.Parent = screenGui

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 30)
titleLabel.Text = "Menu de Auto-Farm"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 20
titleLabel.BackgroundTransparency = 1
titleLabel.Parent = frame

-- Botões para ativar as opções
local autoFarmButton = Instance.new("TextButton")
autoFarmButton.Size = UDim2.new(1, 0, 0, 30)
autoFarmButton.Position = UDim2.new(0, 0, 0, 40)
autoFarmButton.Text = "Ativar Auto-Farm"
autoFarmButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
autoFarmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
autoFarmButton.Parent = frame

local farmBonesButton = Instance.new("TextButton")
farmBonesButton.Size = UDim2.new(1, 0, 0, 30)
farmBonesButton.Position = UDim2.new(0, 0, 0, 80)
farmBonesButton.Text = "Farmar Ossos"
farmBonesButton.BackgroundColor3 = Color3.fromRGB(0, 0, 255)
farmBonesButton.TextColor3 = Color3.fromRGB(255, 255, 255)
farmBonesButton.Parent = frame

local stopButton = Instance.new("TextButton")
stopButton.Size = UDim2.new(1, 0, 0, 30)
stopButton.Position = UDim2.new(0, 0, 0, 120)
stopButton.Text = "Parar"
stopButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
stopButton.TextColor3 = Color3.fromRGB(255, 255, 255)
stopButton.Parent = frame

-- Variáveis para controlar o estado
local autoFarmActive = false
local farmBonesActive = false
local stopActive = false

-- Função para ativar Auto-Farm
local function ativarAutoFarm()
    autoFarmActive = true
    farmBonesActive = false
    stopActive = false
    while autoFarmActive do
        wait(1)
        -- Aqui você pode modificar para atacar inimigos específicos ou coletar itens
        for _, enemy in pairs(workspace.Enemies:GetChildren()) do
            if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
                character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0)
                wait(0.2)
                enemy.Humanoid.Health = enemy.Humanoid.Health - 10
            end
        end
    end
end

-- Função para farmar ossos
local function farmarOssos()
    farmBonesActive = true
    autoFarmActive = false
    stopActive = false
    while farmBonesActive do
        wait(1)
        for _, item in pairs(workspace.Items:GetChildren()) do
            if item.Name == "Bone" and (item.Position - character.HumanoidRootPart.Position).Magnitude < 10 then
                character.HumanoidRootPart.CFrame = CFrame.new(item.Position)
                wait(0.2)
                item:Destroy() -- Coleta o osso
            end
        end
    end
end

-- Função para parar todos os processos
local function parar()
    autoFarmActive = false
    farmBonesActive = false
    stopActive = true
end

-- Conectar os botões aos eventos
autoFarmButton.MouseButton1Click:Connect(function()
    if not autoFarmActive then
        ativarAutoFarm()
    end
end)

farmBonesButton.MouseButton1Click:Connect(function()
    if not farmBonesActive then
        farmarOssos()
    end
end)

stopButton.MouseButton1Click:Connect(function()
    parar()
end)

-- Parar tudo se o jogador sair
player.CharacterAdded:Connect(function()
    autoFarmActive = false
    farmBonesActive = false
    stopActive = true
end)
