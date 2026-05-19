local validIds = {2753915549, 4442272183, 7449423635, 4520749081, 6381829480, 15759515082, 5931540094}
local isBloxFruits = false
for _, id in ipairs(validIds) do
    if game.PlaceId == id then isBloxFruits = true; break end
end
if not isBloxFruits then return end

-- Aguarda carregar
repeat task.wait() until game.Players.LocalPlayer and game.Players.LocalPlayer.Character

-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local TeleportService = game:GetService("TeleportService")
local LocalPlayer = Players.LocalPlayer

-- Config
local Config = {
    AutoFarm = false,
    WalkSpeed = 16,
    JumpPower = 50,
    InfiniteJump = false,
    AntiAFK = false
}

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ItachiHub"
ScreenGui.Parent = game.CoreGui
ScreenGui.ResetOnSpawn = false

-- Ícone flutuante
local ToggleIcon = Instance.new("TextButton")
ToggleIcon.Size = UDim2.new(0, 55, 0, 55)
ToggleIcon.Position = UDim2.new(0, 15, 0.5, -27)
ToggleIcon.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
ToggleIcon.BackgroundTransparency = 0.3
ToggleIcon.Text = "🔴"
ToggleIcon.TextSize = 30
ToggleIcon.Font = Enum.Font.GothamBold
ToggleIcon.BorderSizePixel = 0
ToggleIcon.Parent = ScreenGui
Instance.new("UICorner", ToggleIcon).CornerRadius = UDim.new(1, 0)

-- Painel principal
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 500, 0, 350)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -175)
MainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.Visible = false
MainFrame.Parent = ScreenGui
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

local stroke = Instance.new("UIStroke", MainFrame)
stroke.Color = Color3.fromRGB(255, 0, 0)
stroke.Thickness = 2
stroke.Transparency = 0.5

-- Título
local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 40)
TitleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
TitleBar.BackgroundTransparency = 0.3
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(1, 0, 1, 0)
TitleText.BackgroundTransparency = 1
TitleText.Text = "🔴 ITACHI HUB"
TitleText.TextColor3 = Color3.fromRGB(255, 0, 0)
TitleText.TextSize = 20
TitleText.Font = Enum.Font.GothamBold
TitleText.Parent = TitleBar

-- Botão Toggle
ToggleIcon.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
    ToggleIcon.Text = MainFrame.Visible and "✕" or "🔴"
end)

-- Área de conteúdo
local ContentArea = Instance.new("ScrollingFrame")
ContentArea.Size = UDim2.new(1, -20, 1, -60)
ContentArea.Position = UDim2.new(0, 10, 0, 50)
ContentArea.BackgroundTransparency = 1
ContentArea.BorderSizePixel = 0
ContentArea.ScrollBarThickness = 4
ContentArea.ScrollBarImageColor3 = Color3.fromRGB(255, 0, 0)
ContentArea.CanvasSize = UDim2.new(0, 0, 0, 400)
ContentArea.Parent = MainFrame

-- Criar Toggle
local function CreateToggle(text, yPos, callback)
    local Frame = Instance.new("Frame")
    Frame.Size = UDim2.new(1, 0, 0, 40)
    Frame.Position = UDim2.new(0, 0, 0, yPos)
    Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    Frame.BackgroundTransparency = 0.5
    Frame.BorderSizePixel = 0
    Instance.new("UICorner", Frame).CornerRadius = UDim.new(0, 8)
    Frame.Parent = ContentArea
    
    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(0.7, 0, 1, 0)
    Label.Position = UDim2.new(0, 10, 0, 0)
    Label.BackgroundTransparency = 1
    Label.Text = text
    Label.TextColor3 = Color3.fromRGB(255, 255, 255)
    Label.TextSize = 14
    Label.Font = Enum.Font.Gotham
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Frame
    
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0, 44, 0, 24)
    Button.Position = UDim2.new(1, -55, 0.5, -12)
    Button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Button.Text = ""
    Button.BorderSizePixel = 0
    Button.AutoButtonColor = false
    Instance.new("UICorner", Button).CornerRadius = UDim.new(1, 0)
    
    local Indicator = Instance.new("Frame")
    Indicator.Size = UDim2.new(0, 20, 0, 20)
    Indicator.Position = UDim2.new(0, 2, 0.5, -10)
    Indicator.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    Indicator.BorderSizePixel = 0
    Instance.new("UICorner", Indicator).CornerRadius = UDim.new(1, 0)
    Indicator.Parent = Button
    Button.Parent = Frame
    
    local enabled = false
    Button.MouseButton1Click:Connect(function()
        enabled = not enabled
        callback(enabled)
        if enabled then
            TweenService:Create(Button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(255, 0, 0)}):Play()
            TweenService:Create(Indicator, TweenInfo.new(0.2), {Position = UDim2.new(1, -22, 0.5, -10)}):Play()
        else
            TweenService:Create(Button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
            TweenService:Create(Indicator, TweenInfo.new(0.2), {Position = UDim2.new(0, 2, 0.5, -10)}):Play()
        end
    end)
end

-- Criar Slider
local function CreateSlider(text, min, max, default, yPos, callback)
    local Frame = Instance.new("Frame")
    Frame.Size = UDim2.new(1, 0, 0, 55)
    Frame.Position = UDim2.new(0, 0, 0, yPos)
    Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    Frame.BackgroundTransparency = 0.5
    Frame.BorderSizePixel = 0
    Instance.new("UICorner", Frame).CornerRadius = UDim.new(0, 8)
    Frame.Parent = ContentArea
    
    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, 0, 0, 20)
    Label.Position = UDim2.new(0, 10, 0, 3)
    Label.BackgroundTransparency = 1
    Label.Text = text .. ": " .. default
    Label.TextColor3 = Color3.fromRGB(255, 255, 255)
    Label.TextSize = 12
    Label.Font = Enum.Font.Gotham
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Frame
    
    local Bar = Instance.new("Frame")
    Bar.Size = UDim2.new(1, -40, 0, 4)
    Bar.Position = UDim2.new(0, 20, 0, 30)
    Bar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Bar.BorderSizePixel = 0
    Bar.Parent = Frame
    
    local Fill = Instance.new("Frame")
    Fill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
    Fill.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    Fill.BorderSizePixel = 0
    Fill.Parent = Bar
    
    local Btn = Instance.new("TextButton")
    Btn.Size = UDim2.new(0, 14, 0, 14)
    Btn.Position = UDim2.new((default - min) / (max - min), -7, 0, -5)
    Btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    Btn.Text = ""
    Btn.BorderSizePixel = 0
    Btn.AutoButtonColor = false
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(1, 0)
    Btn.Parent = Bar
    
    local value = default
    local dragConnection
    
    Btn.MouseButton1Down:Connect(function()
        dragConnection = RunService.RenderStepped:Connect(function()
            if Bar.AbsoluteSize.X <= 0 then return end
            local percent = math.clamp((UserInputService:GetMouseLocation().X - Bar.AbsolutePosition.X) / Bar.AbsoluteSize.X, 0, 1)
            value = math.round(min + (max - min) * percent)
            callback(value)
            Fill.Size = UDim2.new(percent, 0, 1, 0)
            Btn.Position = UDim2.new(percent, -7, 0, -5)
            Label.Text = text .. ": " .. value
        end)
    end)
    
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 and dragConnection then
            dragConnection:Disconnect()
        end
    end)
end

-- Adicionar toggles
local y = 0
CreateToggle("Auto Farm", y, function(state) Config.AutoFarm = state end); y = y + 45
CreateToggle("Infinite Jump", y, function(state) Config.InfiniteJump = state end); y = y + 45
CreateToggle("Anti AFK", y, function(state) Config.AntiAFK = state end); y = y + 45

CreateSlider("Walk Speed", 16, 200, 16, y, function(val) Config.WalkSpeed = val end); y = y + 60
CreateSlider("Jump Power", 50, 200, 50, y, function(val) Config.JumpPower = val end); y = y + 60

-- Botão Rejoin
local RejoinBtn = Instance.new("TextButton")
RejoinBtn.Size = UDim2.new(1, 0, 0, 35)
RejoinBtn.Position = UDim2.new(0, 0, 0, y + 10)
RejoinBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
RejoinBtn.BackgroundTransparency = 0.3
RejoinBtn.Text = "🔄 REJOIN"
RejoinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
RejoinBtn.TextSize = 14
RejoinBtn.Font = Enum.Font.GothamBold
RejoinBtn.BorderSizePixel = 0
Instance.new("UICorner", RejoinBtn).CornerRadius = UDim.new(0, 8)
RejoinBtn.Parent = ContentArea
RejoinBtn.MouseButton1Click:Connect(function() TeleportService:Teleport(game.PlaceId) end)

ContentArea.CanvasSize = UDim2.new(0, 0, 0, y + 80)

-- Sistemas
RunService.Heartbeat:Connect(function()
    if Config.AntiAFK then
        pcall(function()
            game:GetService("VirtualUser"):CaptureController()
            game:GetService("VirtualUser"):ClickButton2(Vector2.new())
        end)
    end
    
    if LocalPlayer.Character then
        local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            if Config.WalkSpeed then humanoid.WalkSpeed = Config.WalkSpeed end
            if Config.JumpPower then humanoid.JumpPower = Config.JumpPower end
            if Config.InfiniteJump then humanoid.Jump = true end
        end
    end
    
    if Config.AutoFarm then
        pcall(function()
            local char = LocalPlayer.Character
            if not char then return end
            local nearest = nil
            local nearestDist = 300
            for _, enemy in ipairs(workspace.Enemies:GetChildren()) do
                if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 and enemy:FindFirstChild("HumanoidRootPart") then
                    local dist = (char.HumanoidRootPart.Position - enemy.HumanoidRootPart.Position).Magnitude
                    if dist < nearestDist then nearestDist = dist; nearest = enemy end
                end
            end
            if nearest then
                char.HumanoidRootPart.CFrame = nearest.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0)
                game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, true, game, 1)
                task.wait(0.1)
                game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, false, game, 1)
            end
        end)
    end
end)

print("✅ ItachiHub carregado com sucesso!")
