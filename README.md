-- Inicializando o jogador e a tela
local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

-- Criando o Frame principal para o menu
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 350, 0, 400)
frame.Position = UDim2.new(0.5, -175, 0.5, -200)  -- Colocando o menu no centro da tela
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0.5, 0.5)  -- Centralizando o frame
frame.Parent = screenGui

-- Adicionando bordas arredondadas
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = frame

-- Adicionando título ao menu
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 40)
titleLabel.Text = "xD Hub"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 24
titleLabel.TextStrokeTransparency = 0.8
titleLabel.BackgroundTransparency = 1
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Parent = frame

-- Função para criar botões no menu
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

    -- Definindo o que o botão faz quando clicado
    button.MouseButton1Click:Connect(callback)
    
    return button
end

-- Criando um exemplo de botão de função (aqui você pode adicionar outros)
createButton("Exemplo de Função", UDim2.new(0.1, 0, 0.2, 0), Color3.fromRGB(0, 204, 102), "Ativar Função", function()
    print("Função ativada!")
end)
