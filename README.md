-- Variáveis principais
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Criar uma tela GUI para o menu
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DinoMenu"
screenGui.Parent = PlayerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 400, 0, 600)
frame.Position = UDim2.new(0.5, -200, 0.5, -300)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.5
frame.Parent = screenGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 50)
title.Text = "Dino Menu V1.0 (Brookhaven RP)"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 24
title.BackgroundTransparency = 1
title.Parent = frame

-- Adicionando um dinossauro como logo (pode ser uma imagem URL)
local logo = Instance.new("ImageLabel")
logo.Size = UDim2.new(0, 50, 0, 50)
logo.Position = UDim2.new(0, 10, 0, 10)
logo.Image = "rbxassetid://logo_image_id" -- Substitua pelo ID da sua imagem de dinossauro
logo.Parent = frame

-- Função para abrir/fechar o menu
local function toggleMenu()
    frame.Visible = not frame.Visible
end

-- Fly (Ativar/Desativar)
local flying = false
local bodyGyro, bodyVelocity

local function toggleFly()
    flying = not flying
    if flying then
        -- Criar os objetos necessários para o Fly
        bodyGyro = Instance.new("BodyGyro")
        bodyGyro.MaxTorque = Vector3.new(400000, 400000, 400000)
        bodyGyro.CFrame = player.Character.HumanoidRootPart.CFrame
        bodyGyro.Parent = player.Character.HumanoidRootPart
        
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000)
        bodyVelocity.Velocity = Vector3.new(0, 50, 0)  -- Ajuste a velocidade de voo
        bodyVelocity.Parent = player.Character.HumanoidRootPart
    else
        -- Remover o Fly
        if bodyGyro then bodyGyro:Destroy() end
        if bodyVelocity then bodyVelocity:Destroy() end
    end
end

-- Botão de Fly
local flyButton = Instance.new("TextButton")
flyButton.Size = UDim2.new(0, 200, 0, 50)
flyButton.Position = UDim2.new(0, 100, 0, 100)
flyButton.Text = "Fly (Ativar/Desativar)"
flyButton.BackgroundColor3 = Color3.fromRGB(100, 100, 255)
flyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
flyButton.Parent = frame

flyButton.MouseButton1Click:Connect(toggleFly)

-- ESP (Caixa de ESP)
local function createESP(player)
    local espBox = Instance.new("BoxHandleAdornment")
    espBox.Size = player.Character:GetBoundingBox().Size
    espBox.Adornee = player.Character
    espBox.Color3 = Color3.fromRGB(255, 0, 0)  -- Cor da caixa
    espBox.Parent = player.Character
end

local function toggleESP()
    -- Adicionar a caixa de ESP aos jogadores
    for _, p in ipairs(Players:GetPlayers()) do
        if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            createESP(p)
        end
    end
end

-- Botão de ESP
local espButton = Instance.new("TextButton")
espButton.Size = UDim2.new(0, 200, 0, 50)
espButton.Position = UDim2.new(0, 100, 0, 160)
espButton.Text = "ESP (Ativar/Desativar)"
espButton.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
espButton.TextColor3 = Color3.fromRGB(255, 255, 255)
espButton.Parent = frame

espButton.MouseButton1Click:Connect(toggleESP)

-- Função para modificar o nome e bio
local function setNameAndBio()
    -- Muda a cor do nome e da bio do jogador
    player.DisplayName = "Novo Nome"
    -- Modificar bio no sistema do jogo (se possível, ou apenas ajustar a interface)
    print("Bio alterada!")
end

local nameButton = Instance.new("TextButton")
nameButton.Size = UDim2.new(0, 200, 0, 50)
nameButton.Position = UDim2.new(0, 100, 0, 220)
nameButton.Text = "Nome/Bio Colorido"
nameButton.BackgroundColor3 = Color3.fromRGB(100, 255, 255)
nameButton.TextColor3 = Color3.fromRGB(255, 255, 255)
nameButton.Parent = frame

nameButton.MouseButton1Click:Connect(setNameAndBio)

-- Função para alterar a velocidade do jogador
local speedSlider = Instance.new("Frame")
speedSlider.Size = UDim2.new(0, 200, 0, 20)
speedSlider.Position = UDim2.new(0, 100, 0, 280)
speedSlider.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
speedSlider.Parent = frame

-- Criar um TextButton para atuar como um Slider para velocidade
local speedValue = Instance.new("TextLabel")
speedValue.Size = UDim2.new(0, 40, 0, 20)
speedValue.Position = UDim2.new(1, -50, 0, 0)
speedValue.Text = "50"
speedValue.TextColor3 = Color3.fromRGB(0, 0, 0)
speedValue.BackgroundTransparency = 1
speedValue.Parent = speedSlider

speedSlider.MouseButton1Down:Connect(function()
    speedValue.Text = "100"  -- O código de exemplo mostra um valor fixo. Aqui você pode adicionar lógica para arrastar e ajustar o valor dinamicamente.
end)

-- Função para ativar o super pulo
local jumpSlider = Instance.new("Frame")
jumpSlider.Size = UDim2.new(0, 200, 0, 20)
jumpSlider.Position = UDim2.new(0, 100, 0, 310)
jumpSlider.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
jumpSlider.Parent = frame

local jumpValue = Instance.new("TextLabel")
jumpValue.Size = UDim2.new(0, 40, 0, 20)
jumpValue.Position = UDim2.new(1, -50, 0, 0)
jumpValue.Text = "50"
jumpValue.TextColor3 = Color3.fromRGB(0, 0, 0)
jumpValue.BackgroundTransparency = 1
jumpValue.Parent = jumpSlider

jumpSlider.MouseButton1Down:Connect(function()
    jumpValue.Text = "100"  -- Ajuste a lógica para modificar o valor
end)

-- Função para equipar personagens (exemplo: Korblox)
local function equipCharacterParts()
    -- Aqui você pode adicionar a lógica para equipar partes de personagens, como o Korblox
    print("Equipando partes do personagem!")
end

local characterButton = Instance.new("TextButton")
characterButton.Size = UDim2.new(0, 200, 0, 50)
characterButton.Position = UDim2.new(0, 100, 0, 340)
characterButton.Text = "Equipar Personagem"
characterButton.BackgroundColor3 = Color3.fromRGB(150, 150, 255)
characterButton.TextColor3 = Color3.fromRGB(255, 255, 255)
characterButton.Parent = frame

characterButton.MouseButton1Click:Connect(equipCharacterParts)

-- Finalizando com a função de minimizar/abrir o menu
local function minimizeMenu()
    frame.Visible = false
end

local minimizeButton = Instance.new("TextButton")
minimizeButton.Size = UDim2.new(0, 50, 0, 50)
minimizeButton.Position = UDim2.new(1, -50, 0, 10)
minimizeButton.Text = "-"
minimizeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
minimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeButton.Parent = frame

minimizeButton.MouseButton1Click:Connect(minimizeMenu)
