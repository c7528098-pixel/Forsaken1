--[[
    Script: Painel de Controle Simples com Créditos
    Créditos: chr_apns
    Propósito: Educacional - Demonstrar a criação de uma GUI arrastável e minimizável.
    Aviso: O uso de exploits viola os Termos de Serviço da Roblox e pode resultar em banimento.
]]

-- Evita que o script crie múltiplos painéis se for executado mais de uma vez
if getgenv().ControlPanelLoaded then return end
getgenv().ControlPanelLoaded = true

--================================================================
-- CRIAÇÃO DA INTERFACE (GUI)
--================================================================

-- O ScreenGui é o contêiner principal que fica na tela
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ControlPanelScreenGui"
ScreenGui.Parent = game:GetService("CoreGui") -- Usamos CoreGui para persistência
ScreenGui.ResetOnSpawn = false

-- O Frame principal, nosso painel vermelho
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 220, 0, 180) -- Aumentei um pouco a altura para os créditos
MainFrame.Position = UDim2.new(0, 20, 0, 20)
MainFrame.BackgroundColor3 = Color3.fromRGB(200, 30, 30) -- Vermelho
MainFrame.BorderColor3 = Color3.fromRGB(0, 0, 0)
MainFrame.BorderSizePixel = 2
MainFrame.Parent = ScreenGui

-- A barra de título, usada para arrastar e mostrar o nome
local TitleBar = Instance.new("TextLabel")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 30)
TitleBar.BackgroundColor3 = Color3.fromRGB(150, 20, 20) -- Vermelho mais escuro
TitleBar.Text = "Painel de Controle"
TitleBar.Font = Enum.Font.SourceSansBold
TitleBar.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleBar.TextSize = 18
TitleBar.Parent = MainFrame

-- Botão para minimizar e maximizar o painel
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, 25, 0, 25)
MinimizeButton.Position = UDim2.new(1, -28, 0, 3)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(200, 30, 30)
MinimizeButton.BorderColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.Text = "-"
MinimizeButton.Font = Enum.Font.SourceSansBold
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 24
MinimizeButton.Parent = TitleBar

-- Frame que contém os botões de função
local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "ContentFrame"
ContentFrame.Size = UDim2.new(1, 0, 1, -30)
ContentFrame.Position = UDim2.new(0, 0, 0, 30)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = MainFrame

--================================================================
-- LÓGICA DA INTERFACE
--================================================================

-- Lógica para arrastar o painel
local UserInputService = game:GetService("UserInputService")
local dragging = false
local dragStart
local startPos

TitleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
    end
end)

TitleBar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

-- Lógica para minimizar o painel
local isMinimized = false
MinimizeButton.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    ContentFrame.Visible = not isMinimized
    if isMinimized then
        MainFrame.Size = UDim2.new(0, 220, 0, 30) -- Tamanho só da barra de título
        MinimizeButton.Text = "+"
    else
        MainFrame.Size = UDim2.new(0, 220, 0, 180) -- Tamanho original
        MinimizeButton.Text = "-"
    end
end)


--================================================================
-- BOTÕES DE FUNÇÃO E LÓGICA
--================================================================

-- Tabela global para guardar as configurações
getgenv().Settings = {
    Aimbot = false,
    ESP = false,
    Walkspeed = false,
    OriginalWalkspeed = nil,
    WalkspeedValue = 50 -- Velocidade que o jogador terá
}

-- Função para criar um botão de ativação
local function CreateToggleButton(text, yPosition, featureName)
    local button = Instance.new("TextButton")
    button.Name = featureName .. "Button"
    button.Size = UDim2.new(1, -20, 0, 30)
    button.Position = UDim2.new(0, 10, 0, yPosition)
    button.BackgroundColor3 = Color3.fromRGB(150, 20, 20)
    button.BorderColor3 = Color3.fromRGB(255, 255, 255)
    button.Text = text .. " [OFF]"
    button.Font = Enum.Font.SourceSans
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 16
    button.Parent = ContentFrame

    button.MouseButton1Click:Connect(function()
        -- Inverte o estado da função (liga/desliga)
        getgenv().Settings[featureName] = not getgenv().Settings[featureName]

        -- Atualiza a aparência do botão
        if getgenv().Settings[featureName] then
            button.Text = text .. " [ON]"
            button.BackgroundColor3 = Color3.fromRGB(30, 180, 30) -- Verde
        else
            button.Text = text .. " [OFF]"
            button.BackgroundColor3 = Color3.fromRGB(150, 20, 20) -- Vermelho escuro
        end
    end)
    return button
end

-- Cria os botões
CreateToggleButton("Aimbot", 10, "Aimbot")
CreateToggleButton("ESP", 50, "ESP")
CreateToggleButton("Walkspeed", 90, "Walkspeed")

-- >>> NOVO CÓDIGO AQUI <<<
-- Label de Créditos
local CreditsLabel = Instance.new("TextLabel")
CreditsLabel.Name = "CreditsLabel"
CreditsLabel.Size = UDim2.new(1, -20, 0, 20)
CreditsLabel.Position = UDim2.new(0, 10, 0, 125) -- Posição na parte de baixo
CreditsLabel.BackgroundTransparency = 1
CreditsLabel.Font = Enum.Font.SourceSans
CreditsLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
CreditsLabel.TextSize = 14
CreditsLabel.Text = "Créditos: chr_apns"
CreditsLabel.TextXAlignment = Enum.TextXAlignment.Right -- Alinhado à direita
CreditsLabel.Parent = ContentFrame
-- >>> FIM DO NOVO CÓDIGO <<<

--================================================================
-- EXECUÇÃO DAS FUNÇÕES (PLACEHOLDERS)
--================================================================
-- Esta seção é onde a lógica das funções rodaria em um loop

local player = game.Players.LocalPlayer

game:GetService("RunService").RenderStepped:Connect(function()
    -- Lógica do Walkspeed
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        local humanoid = player.Character.Humanoid
        if getgenv().Settings.Walkspeed then
            if not getgenv().Settings.OriginalWalkspeed then
                getgenv().Settings.OriginalWalkspeed = humanoid.WalkSpeed
            end
            humanoid.WalkSpeed = getgenv().Settings.WalkspeedValue
        else
            if getgenv().Settings.OriginalWalkspeed then
                humanoid.WalkSpeed = getgenv().Settings.OriginalWalkspeed
                getgenv().Settings.OriginalWalkspeed = nil
            end
        end
    end
end)

print("Painel de Controle (Créditos: chr_apns) carregado.")# Forsaken1
