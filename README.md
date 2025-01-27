-- Configuração inicial
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local workspace = game.Workspace
local tweenService = game:GetService("TweenService")
local mouse = player:GetMouse()

-- Criando a interface GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 350, 0, 400)
frame.Position = UDim2.new(0, 50, 0, 50)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0, 0)
frame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = frame

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 40)
titleLabel.Text = "Auto-Farm Menu"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 24
titleLabel.TextStrokeTransparency = 0.8
titleLabel.BackgroundTransparency = 1
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Parent = frame

-- Função para criar botões dinâmicos
local function createButton(name, position, color, text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.8, 0, 0, 40)
    button.Position = position
    button.Text = text
    button.BackgroundColor3 = color
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 18
    button.Font = Enum.Font.Gotham
    button.BorderRadius = UDim.new(0, 12)
    button.Parent = frame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = button

    -- Animação de botão ao passar o mouse
    button.MouseEnter:Connect(function()
        local tweenInfo = TweenInfo.new(0.1, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)
        local goal = {BackgroundColor3 = button.BackgroundColor3:Lerp(Color3.fromRGB(255, 255, 255), 0.1)}
        local tween = tweenService:Create(button, tweenInfo, goal)
        tween:Play()
    end)

    -- Definindo a ação do botão
    button.MouseButton1Click:Connect(callback)
    
    return button
end

-- Funções de Auto-Farm, Coleta e Parada
local autoFarmActive = false
local farmBonesActive = false
local stopActive = false

-- Função para ativar o Auto-Farm
local function startAutoFarm()
    autoFarmActive = true
    farmBonesActive = false
    stopActive = false
    while autoFarmActive do
        wait(1)
        -- Lógica de auto-farm: buscar inimigos e atacar
        for _, enemy in pairs(workspace.Enemies:GetChildren()) do
            if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
                -- Apontar para o inimigo e dar dano
                character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0)
                wait(0.2)
                enemy.Humanoid.Health = enemy.Humanoid.Health - 10
            end
        end
    end
end

-- Função para farmar ossos
local function farmBones()
    farmBonesActive = true
    autoFarmActive = false
    stopActive = false
    while farmBonesActive do
        wait(1)
        -- Lógica para pegar ossos no jogo
        for _, item in pairs(workspace.Items:GetChildren()) do
            if item.Name == "Bone" and (item.Position - character.HumanoidRootPart.Position).Magnitude < 10 then
                character.HumanoidRootPart.CFrame = CFrame.new(item.Position)
                wait(0.2)
                item:Destroy() -- Coleta o osso
            end
        end
    end
end

-- Função para parar todas as atividades
local function stopAll()
    autoFarmActive = false
    farmBonesActive = false
    stopActive = true
end

-- Criando os botões do menu
createButton("Ativar Auto-Farm", UDim2.new(0.1, 0, 0.2, 0), Color3.fromRGB(0, 204, 102), "Ativar Auto-Farm", function()
    if not autoFarmActive then
        startAutoFarm()
    end
end)

createButton("Farmar Ossos", UDim2.new(0.1, 0, 0.35, 0), Color3.fromRGB(0, 153, 255), "Farmar Ossos", function()
    if not farmBonesActive then
        farmBones()
    end
end)

createButton("Parar Tudo", UDim2.new(0.1, 0, 0.5, 0), Color3.fromRGB(255, 77, 77), "Parar Tudo", function()
    stopAll()
end)

-- Função para quando o jogador reiniciar
player.CharacterAdded:Connect(function()
    stopAll()
end)
