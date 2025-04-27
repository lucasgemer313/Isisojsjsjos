-- Aimbot Hub - Mira Vertical Ajustável e Centralizado

-- Hub Principal
local ScreenGuiHub = Instance.new("ScreenGui")
local OpenButton = Instance.new("TextButton")

-- Tela de Configuração
local ScreenGuiConfig = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local MiraPosLabel = Instance.new("TextLabel")
local MiraPosBox = Instance.new("TextBox")
local FOVLabel = Instance.new("TextLabel")
local FOVBox = Instance.new("TextBox")
local TargetPartButton = Instance.new("TextButton")
local ToggleButton = Instance.new("TextButton")
local FOVCircle = Drawing.new("Circle")

-- Variáveis
local aimbotAtivo = false
local miraVertical = 100 -- 0 a 200
local fovTamanho = 200 -- 1 a 400
local configAberta = false
local targetPart = "Head" -- Mira inicial na cabeça

-- Configurações do Hub
ScreenGuiHub.Name = "AimbotHub"
ScreenGuiHub.Parent = game.CoreGui

OpenButton.Parent = ScreenGuiHub
OpenButton.Size = UDim2.new(0, 120, 0, 50)
OpenButton.Position = UDim2.new(0, 20, 0.5, -25)
OpenButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
OpenButton.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenButton.Font = Enum.Font.SourceSansBold
OpenButton.Text = "Aimbot"
OpenButton.TextSize = 22

-- Configurações da Tela
ScreenGuiConfig.Name = "AimbotConfig"
ScreenGuiConfig.Parent = game.CoreGui
ScreenGuiConfig.Enabled = false

Frame.Parent = ScreenGuiConfig
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.Position = UDim2.new(0.5, -150, 0.5, -200)
Frame.Size = UDim2.new(0, 300, 0, 450)
Frame.BorderSizePixel = 0

Title.Parent = Frame
Title.Text = "Config. de Mira"
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 28
Title.Size = UDim2.new(1, 0, 0, 50)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1

MiraPosLabel.Parent = Frame
MiraPosLabel.Position = UDim2.new(0, 0, 0, 60)
MiraPosLabel.Size = UDim2.new(1, 0, 0, 25)
MiraPosLabel.Text = "Posição Vertical (0-200)"
MiraPosLabel.Font = Enum.Font.SourceSans
MiraPosLabel.TextSize = 18
MiraPosLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
MiraPosLabel.BackgroundTransparency = 1

MiraPosBox.Parent = Frame
MiraPosBox.Position = UDim2.new(0.1, 0, 0, 90)
MiraPosBox.Size = UDim2.new(0.8, 0, 0, 40)
MiraPosBox.Text = tostring(miraVertical)
MiraPosBox.Font = Enum.Font.SourceSans
MiraPosBox.TextSize = 22
MiraPosBox.TextColor3 = Color3.fromRGB(0, 0, 0)
MiraPosBox.BackgroundColor3 = Color3.fromRGB(200, 200, 200)

FOVLabel.Parent = Frame
FOVLabel.Position = UDim2.new(0, 0, 0, 150)
FOVLabel.Size = UDim2.new(1, 0, 0, 25)
FOVLabel.Text = "Tamanho do FOV (1-400)"
FOVLabel.Font = Enum.Font.SourceSans
FOVLabel.TextSize = 18
FOVLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
FOVLabel.BackgroundTransparency = 1

FOVBox.Parent = Frame
FOVBox.Position = UDim2.new(0.1, 0, 0, 180)
FOVBox.Size = UDim2.new(0.8, 0, 0, 40)
FOVBox.Text = tostring(fovTamanho)
FOVBox.Font = Enum.Font.SourceSans
FOVBox.TextSize = 22
FOVBox.TextColor3 = Color3.fromRGB(0, 0, 0)
FOVBox.BackgroundColor3 = Color3.fromRGB(200, 200, 200)

TargetPartButton.Parent = Frame
TargetPartButton.Position = UDim2.new(0.1, 0, 0, 250)
TargetPartButton.Size = UDim2.new(0.8, 0, 0, 50)
TargetPartButton.Text = "Mira: Cabeça"
TargetPartButton.Font = Enum.Font.SourceSansBold
TargetPartButton.TextSize = 22
TargetPartButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TargetPartButton.BackgroundColor3 = Color3.fromRGB(50, 150, 200)

ToggleButton.Parent = Frame
ToggleButton.Position = UDim2.new(0.1, 0, 0, 320)
ToggleButton.Size = UDim2.new(0.8, 0, 0, 50)
ToggleButton.Text = "Ativar Aimbot: OFF"
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.TextSize = 24
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)

-- Círculo do FOV
FOVCircle.Color = Color3.fromRGB(0, 255, 255)
FOVCircle.Thickness = 2
FOVCircle.Transparency = 0.5
FOVCircle.Filled = false
FOVCircle.Visible = true
FOVCircle.Radius = fovTamanho / 2

-- Funções da Hub
OpenButton.MouseButton1Click:Connect(function()
    configAberta = not configAberta
    ScreenGuiConfig.Enabled = configAberta
end)

MiraPosBox.FocusLost:Connect(function()
    local val = tonumber(MiraPosBox.Text)
    if val and val >= 0 and val <= 200 then
        miraVertical = val
    else
        MiraPosBox.Text = tostring(miraVertical)
    end
end)

FOVBox.FocusLost:Connect(function()
    local val = tonumber(FOVBox.Text)
    if val and val >= 1 and val <= 400 then
        fovTamanho = val
        FOVCircle.Radius = fovTamanho / 2
    else
        FOVBox.Text = tostring(fovTamanho)
    end
end)

TargetPartButton.MouseButton1Click:Connect(function()
    if targetPart == "Head" then
        targetPart = "UpperTorso"
        TargetPartButton.Text = "Mira: Torso"
    else
        targetPart = "Head"
        TargetPartButton.Text = "Mira: Cabeça"
    end
end)

ToggleButton.MouseButton1Click:Connect(function()
    aimbotAtivo = not aimbotAtivo
    ToggleButton.Text = "Ativar Aimbot: " .. (aimbotAtivo and "ON" or "OFF")
    ToggleButton.BackgroundColor3 = aimbotAtivo and Color3.fromRGB(50, 200, 50) or Color3.fromRGB(200, 50, 50)
end)

-- Função Aimbot
game:GetService("RunService").RenderStepped:Connect(function()
    local camera = workspace.CurrentCamera
    local screenCenterX = camera.ViewportSize.X / 2
    local screenCenterY = camera.ViewportSize.Y * (miraVertical / 200)
    FOVCircle.Position = Vector2.new(screenCenterX, screenCenterY)

    if aimbotAtivo then
        local players = game.Players:GetPlayers()
        local closest = nil
        local minDist = fovTamanho

        for _, plr in ipairs(players) do
            if plr ~= game.Players.LocalPlayer and plr.Character and plr.Character:FindFirstChild(targetPart) then
                local partPos, visible = camera:WorldToViewportPoint(plr.Character[targetPart].Position)
                if visible then
                    local targetPos = Vector2.new(screenCenterX, screenCenterY)
                    local dist = (Vector2.new(partPos.X, partPos.Y) - targetPos).Magnitude
                    if dist < minDist then
                        closest = plr.Character[targetPart]
                        minDist = dist
                    end
                end
            end
        end

        if closest then
            camera.CFrame = CFrame.new(camera.CFrame.Position, closest.Position)
        end
    end
end)
