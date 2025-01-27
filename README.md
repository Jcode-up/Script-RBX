-- Configuração inicial do script
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local workspace = game.Workspace
local httpService = game:GetService("HttpService")

-- Criando o menu GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 200)
frame.Position = UDim2.new(0, 20, 0, 20)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.7
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0, 0)
frame.Parent = screenGui

-- Adicionando um canto arredondado
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 20)
corner.Parent = frame

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 40)
titleLabel.Text = "Menu de Auto-Farm"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 24
titleLabel.Font = Enum.Font.GothamBold
titleLabel.BackgroundTransparency = 1
titleLabel.Parent = frame

-- Botões para ativar as opções
local autoFarmButton = Instance.new("TextButton")
autoFarmButton.Size = UDim2.new(0.8, 0, 0, 40)
autoFarmButton.Position = UDim2.new(0.1, 0, 0.2, 0)
autoFarmButton.Text = "Ativar Auto-Farm"
autoFarmButton.BackgroundColor3 = Color3.fromRGB(0, 204, 102)
autoFarmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
autoFarmButton.TextSize = 18
autoFarmButton.Font = Enum.Font.Gotham
autoFarmButton.BorderRadius = UDim.new(0, 12)
autoFarmButton.Parent = frame

local farmBonesButton = Instance.new("TextButton")
farmBonesButton.Size = UDim2.new(0.8, 0, 0, 40)
farmBonesButton.Position = UDim2.new(0.1, 0, 0.4, 0)
farmBonesButton.Text = "Farmar Ossos"
farmBonesButton.BackgroundColor3 = Color3.fromRGB(0, 153, 255)
farmBonesButton.TextColor3 = Color3.fromRGB(255, 255, 255)
farmBonesButton.TextSize = 18
farmBonesButton.Font = Enum.Font.Gotham
farmBonesButton.BorderRadius = UDim.new(0, 12)
farmBonesButton.Parent = frame

local stopButton = Instance.new("TextButton")
stopButton.Size = UDim2.new(0.8, 0, 0, 40)
stopButton.Position = UDim2.new(0.1, 0, 0.6, 0)
stopButton.Text = "Parar"
stopButton.BackgroundColor3 = Color3.fromRGB(255, 77, 77)
stopButton.TextColor3 = Color3.fromRGB(255, 255, 255)
stopButton.TextSize = 18
stopButton.Font = Enum.Font.Gotham
stopButton.BorderRadius = UDim.new(0, 12)
stopButton.Parent = frame

-- Animações para os botões
local function animaBotao(button)
    local tweenService = game:GetService("TweenService")
    local tweenInfo = TweenInfo.new(0.2, Enum.EasingStyle.Quart, Enum.EasingDirection.Out)
    
    local goal = {}
    goal.BackgroundColor3 = button.BackgroundColor3:lerp(Color3.fromRGB(255, 255, 255), 0.1) -- Efeito de brilho
    local tween = tweenService:Create(button, tweenInfo, goal)
    
    tween:Play()
    tween.Completed:Connect(function()
        goal.BackgroundColor3 = button.BackgroundColor3:lerp(Color3.fromRGB(255, 255, 255), 0)
        tween:Play()
    end)
end

autoFarmButton.MouseEnter:Connect(function()
    animaBotao(autoFarmButton)
end)
farmBonesButton.MouseEnter:Connect(function()
    animaBotao(farmBonesButton)
end)
stopButton.MouseEnter:Connect(function()
    animaBotao(stopButton)
end)

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
