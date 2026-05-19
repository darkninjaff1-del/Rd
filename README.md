--[[
    🔴 ITACHI HUB - BLOX FRUITS ULTIMATE SECURE
    Dark/Anime Theme | Itachi Uchiha Inspired
    Version 5.0.0 | Anti-Ban + Secure
--]]

local success, err = pcall(function()

local function VerifyPlace()
    local validPlaces = {2753915549, 4442272183, 7449423635, 4520749081, 6381829480, 15759515082, 5931540094}
    for _, id in ipairs(validPlaces) do if game.PlaceId == id then return true end end
    return false
end
if not VerifyPlace() then return end

local function WaitForDataLoaded()
    local start = tick()
    local dataLoaded = false
    repeat
        task.wait()
        if tick() - start > 60 then warn("❌ Timeout"); return false end
        pcall(function()
            if game.Players.LocalPlayer and game.Players.LocalPlayer:FindFirstChild("DataLoaded") and game.Players.LocalPlayer:FindFirstChild("DataLoaded").Value then
                dataLoaded = true
            end
        end)
    until dataLoaded
    return true
end
if not WaitForDataLoaded() then return end

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local Lighting = game:GetService("Lighting")
local LocalPlayer = Players.LocalPlayer

local ScriptName = "ItachiHub"
local ScriptVersion = "v5.0.0"
local LastAction = 0
local ActionCooldown = 0.1

local function SecureRandom(min, max) return math.random() * (max - min) + min end
local function AntiDetectionCooldown()
    local elapsed = tick() - LastAction
    if elapsed < ActionCooldown then task.wait(ActionCooldown - elapsed) end
    LastAction = tick()
end
local function GetRandomOffset() return Vector3.new(SecureRandom(-2, 2), SecureRandom(-1, 3), SecureRandom(-2, 2)) end

local Config = {
    Farm = {AutoFarm = false, AutoQuest = false, FastAttack = false, AutoBoss = false, AutoStats = false, AutoEquipBestSword = false, AutoCollectFruits = false},
    Raid = {AutoStartRaid = false, AutoAwaken = false, AutoBuyChip = false},
    Fruit = {AutoFindFruit = false, FruitSniper = false, AutoStoreInInventory = false, AutoSpinFruit = false},
    Shop = {AutoBuyHaki = false, AutoBuyGeppo = false, AutoBuySoru = false, AutoBuyKen = false, AutoBuyBuso = false},
    Mastery = {AutoFarmMastery = false, AutoSwitchWeapon = false, TargetMasteryLevel = 600},
    Teleport = {AutoTPToQuest = false, AutoTPToBoss = false, TpToSeaBeast = false, TpToChest = false},
    Boss = {AutoJoinBossRoom = false, AutoLeaveAfterKill = false, BossHop = false, AutoDarkBeard = false},
    SeaEvents = {AutoSeaBeast = false, AutoShipRaid = false, AutoKraken = false, AutoFactory = false},
    Factory = {AutoFactoryEvent = false, AutoCollectMaterial = false},
    Combat = {AutoClick = false, CPS = 10, FastClick = false, AutoHaki = false, AutoBusoHaki = false, AutoKenHaki = false, AutoEquipWeapon = false, KillAura = false, NoStun = false},
    Stats = {AutoDistributeStats = false, PointsPerStat = 3},
    Visual = {ESP = false, ESPPlayers = false, ESPFruits = false, ESPChests = false, FullBright = false, NoClip = false, RemoveFog = false},
    PvP = {AimFOV = 200, ShowFOV = false, Aimbot = false, AimAssist = false, AllyCheck = false, TeamCheck = false, AutoBounty = false, SafeZone = false},
    Player = {WalkSpeed = 16, JumpPower = 50, InfiniteJump = false, Fly = false, FlySpeed = 50, GodMode = false},
    Race = {AutoEvolveRace = false, AutoBuyRaceReroll = false},
    Crew = {AutoJoinCrew = false, AutoAcceptCrewQuest = false},
    Quest = {AutoTalkNPC = false, AutoCompleteQuest = false, AutoTurnInQuest = false, SkipQuestDialog = false},
    Notifications = {NotifyOnFruitSpawn = false, NotifyOnBossSpawn = false, NotifyOnSeaBeast = false, WebhookURL = ""},
    Misc = {AntiAFK = false, FPSBoost = false, RemoveTerrain = false, AutoRedeemCodes = false, AutoOpenChests = false, AutoCollectGems = false, AutoCollectBeli = false},
    Security = {AutoLeaveOnAdmin = false, AntiTPBack = false, HideCharacter = false, SafeMode = true, MaxCPSCap = 15, MaxSpeedCap = 200, AutoResetOnBan = false}
}

local function SaveConfig()
    pcall(function()
        if writefile and isfolder and makefolder then
            if not isfolder("ItachiHub_Secure") then makefolder("ItachiHub_Secure") end
            writefile("ItachiHub_Secure/config_secure.json", HttpService:JSONEncode(Config))
        end
    end)
end

local function LoadConfig()
    pcall(function()
        if readfile and isfile and isfile("ItachiHub_Secure/config_secure.json") then
            local loaded = HttpService:JSONDecode(readfile("ItachiHub_Secure/config_secure.json"))
            for cat, settings in pairs(loaded) do
                if Config[cat] then for key, val in pairs(settings) do if Config[cat][key] ~= nil then Config[cat][key] = val end end end
            end
        end
    end)
end

local ItachiHub = {
    Flags = {}, Connections = {},
    Theme = {
        Primary = Color3.fromRGB(255, 0, 0), Secondary = Color3.fromRGB(180, 0, 0), Accent = Color3.fromRGB(255, 50, 50),
        Background = Color3.fromRGB(10, 10, 10), Surface = Color3.fromRGB(20, 20, 20), SurfaceLight = Color3.fromRGB(30, 30, 30),
        Text = Color3.fromRGB(255, 255, 255), TextDim = Color3.fromRGB(150, 150, 150), Success = Color3.fromRGB(0, 255, 100)
    }
}

local BF = {}
function BF:GetCurrentWorld()
    local id = game.PlaceId
    if id == 2753915549 or id == 4442272183 or id == 7449423635 then return "First Sea"
    elseif id == 4520749081 or id == 6381829480 then return "Second Sea"
    elseif id == 15759515082 or id == 5931540094 then return "Third Sea" end
    return "Unknown"
end
function BF:GetCharacterSafe()
    if not LocalPlayer then return nil end
    local char = LocalPlayer.Character
    if not char then return nil end
    if not char:FindFirstChild("HumanoidRootPart") then return nil end
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hum or hum.Health <= 0 then return nil end
    return char
end
function BF:GetNearestEnemy(range)
    local char = BF:GetCharacterSafe()
    if not char then return nil end
    local nearest, nearestDist = nil, range or 300
    pcall(function()
        for _, v in ipairs(workspace.Enemies:GetChildren()) do
            if v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 and v:FindFirstChild("HumanoidRootPart") then
                local dist = (char.HumanoidRootPart.Position - v.HumanoidRootPart.Position).Magnitude
                if dist < nearestDist then nearestDist = dist; nearest = v end
            end
        end
    end)
    return nearest
end
function BF:ClickSecure()
    pcall(function()
        AntiDetectionCooldown()
        game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, true, game, 1)
        task.wait(SecureRandom(0.05, 0.1))
        game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, false, game, 1)
    end)
end

local AntiBan = {}
function AntiBan:SafeCFrame(pos) return CFrame.new(pos + GetRandomOffset()) end
function AntiBan:ValidateSpeed(speed) return Config.Security.SafeMode and math.min(speed, Config.Security.MaxSpeedCap) or speed end
function AntiBan:ValidateCPS(cps) return Config.Security.SafeMode and math.min(cps, Config.Security.MaxCPSCap) or cps end

local function CreateGUI()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "ItachiHub_Secure"
    ScreenGui.Parent = game.CoreGui
    ScreenGui.ResetOnSpawn = false
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    local ToggleIcon = Instance.new("TextButton")
    ToggleIcon.Size = UDim2.new(0, 55, 0, 55)
    ToggleIcon.Position = UDim2.new(0, 15, 0.5, -27)
    ToggleIcon.BackgroundColor3 = ItachiHub.Theme.Primary
    ToggleIcon.BackgroundTransparency = 0.3
    ToggleIcon.Text = "🔴"
    ToggleIcon.TextSize = 30
    ToggleIcon.Font = Enum.Font.GothamBold
    ToggleIcon.BorderSizePixel = 0
    ToggleIcon.ZIndex = 10
    ToggleIcon.Parent = ScreenGui
    Instance.new("UICorner", ToggleIcon).CornerRadius = UDim.new(1, 0)
    local ts = Instance.new("UIStroke", ToggleIcon); ts.Color = Color3.fromRGB(255, 255, 255); ts.Thickness = 2; ts.Transparency = 0.5

    local MainFrame = Instance.new("Frame")
    MainFrame.Size = UDim2.new(0, 650, 0, 450)
    MainFrame.Position = UDim2.new(0.5, -325, 0.5, -225)
    MainFrame.BackgroundColor3 = ItachiHub.Theme.Background
    MainFrame.BackgroundTransparency = 0.1
    MainFrame.BorderSizePixel = 0
    MainFrame.ClipsDescendants = true
    MainFrame.Visible = false
    MainFrame.Parent = ScreenGui
    Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)
    local ms = Instance.new("UIStroke", MainFrame); ms.Color = ItachiHub.Theme.Primary; ms.Thickness = 2; ms.Transparency = 0.5

    local uiVisible = false
    ToggleIcon.MouseButton1Click:Connect(function() uiVisible = not uiVisible; MainFrame.Visible = uiVisible; ToggleIcon.Text = uiVisible and "✕" or "🔴" end)

    local TitleBar = Instance.new("Frame")
    TitleBar.Size = UDim2.new(1, 0, 0, 40)
    TitleBar.BackgroundColor3 = ItachiHub.Theme.Surface
    TitleBar.BackgroundTransparency = 0.3
    TitleBar.BorderSizePixel = 0
    TitleBar.Parent = MainFrame
    Instance.new("UICorner", TitleBar).CornerRadius = UDim.new(0, 12)
    
    local TitleLabel = Instance.new("TextLabel")
    TitleLabel.Size = UDim2.new(0, 400, 1, 0)
    TitleLabel.Position = UDim2.new(0, 15, 0, 0)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = "🔴 ITACHI HUB | " .. BF:GetCurrentWorld() .. " | SECURE"
    TitleLabel.TextColor3 = ItachiHub.Theme.Primary
    TitleLabel.TextSize = 16
    TitleLabel.Font = Enum.Font.GothamBold
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    TitleLabel.Parent = TitleBar

    local TabContainer = Instance.new("Frame")
    TabContainer.Size = UDim2.new(0, 150, 1, -50)
    TabContainer.Position = UDim2.new(0, 0, 0, 50)
    TabContainer.BackgroundColor3 = ItachiHub.Theme.Surface
    TabContainer.BackgroundTransparency = 0.5
    TabContainer.BorderSizePixel = 0
    TabContainer.Parent = MainFrame
    
    local ContentFrame = Instance.new("Frame")
    ContentFrame.Size = UDim2.new(1, -160, 1, -60)
    ContentFrame.Position = UDim2.new(0, 155, 0, 55)
    ContentFrame.BackgroundTransparency = 1
    ContentFrame.ClipsDescendants = true
    ContentFrame.Parent = MainFrame

    local Tabs = {"Farm", "Combat", "PvP", "Player", "Visual", "Misc"}
    local TabButtons = {}
    local TabContents = {}

    for i, tabName in ipairs(Tabs) do
        local TabButton = Instance.new("TextButton")
        TabButton.Size = UDim2.new(1, -10, 0, 35)
        TabButton.Position = UDim2.new(0, 5, 0, 5 + (i-1) * 40)
        TabButton.BackgroundColor3 = ItachiHub.Theme.SurfaceLight
        TabButton.BackgroundTransparency = 0.7
        TabButton.Text = tabName
        TabButton.TextColor3 = ItachiHub.Theme.TextDim
        TabButton.TextSize = 13
        TabButton.Font = Enum.Font.GothamBold
        TabButton.BorderSizePixel = 0
        Instance.new("UICorner", TabButton).CornerRadius = UDim.new(0, 6)
        TabButton.Parent = TabContainer
        TabButtons[tabName] = TabButton
        
        local TabContent = Instance.new("ScrollingFrame")
        TabContent.Size = UDim2.new(1, -10, 1, -10)
        TabContent.Position = UDim2.new(0, 5, 0, 5)
        TabContent.BackgroundTransparency = 1
        TabContent.BorderSizePixel = 0
        TabContent.ScrollBarThickness = 4
        TabContent.ScrollBarImageColor3 = ItachiHub.Theme.Primary
        TabContent.Visible = (i == 1)
        TabContent.CanvasSize = UDim2.new(0, 0, 0, 0)
        TabContent.Parent = ContentFrame
        TabContents[tabName] = TabContent
        
        TabButton.MouseButton1Click:Connect(function()
            for _, btn in pairs(TabButtons) do btn.BackgroundColor3 = ItachiHub.Theme.SurfaceLight; btn.BackgroundTransparency = 0.7; btn.TextColor3 = ItachiHub.Theme.TextDim end
            TabButton.BackgroundColor3 = ItachiHub.Theme.Primary; TabButton.BackgroundTransparency = 0.3; TabButton.TextColor3 = ItachiHub.Theme.Text
            for _, content in pairs(TabContents) do content.Visible = false end
            TabContent.Visible = true
        end)
    end
    TabButtons["Farm"].BackgroundColor3 = ItachiHub.Theme.Primary; TabButtons["Farm"].BackgroundTransparency = 0.3; TabButtons["Farm"].TextColor3 = ItachiHub.Theme.Text

    local function CreateToggle(category, name, parent, yPos)
        local ToggleFrame = Instance.new("Frame")
        ToggleFrame.Size = UDim2.new(1, -10, 0, 35)
        ToggleFrame.Position = UDim2.new(0, 5, 0, yPos)
        ToggleFrame.BackgroundColor3 = ItachiHub.Theme.Surface
        ToggleFrame.BackgroundTransparency = 0.5
        ToggleFrame.BorderSizePixel = 0
        Instance.new("UICorner", ToggleFrame).CornerRadius = UDim.new(0, 6)
        
        local ToggleLabel = Instance.new("TextLabel")
        ToggleLabel.Size = UDim2.new(0.65, 0, 1, 0)
        ToggleLabel.Position = UDim2.new(0, 8, 0, 0)
        ToggleLabel.BackgroundTransparency = 1
        ToggleLabel.Text = name
        ToggleLabel.TextColor3 = ItachiHub.Theme.Text
        ToggleLabel.TextSize = 12
        ToggleLabel.Font = Enum.Font.Gotham
        ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
        ToggleLabel.Parent = ToggleFrame
        
        local ToggleButton = Instance.new("TextButton")
        ToggleButton.Size = UDim2.new(0, 40, 0, 22)
        ToggleButton.Position = UDim2.new(1, -48, 0.5, -11)
        ToggleButton.BackgroundColor3 = Config[category] and Config[category][name] and ItachiHub.Theme.Primary or ItachiHub.Theme.SurfaceLight
        ToggleButton.Text = ""
        ToggleButton.BorderSizePixel = 0
        ToggleButton.AutoButtonColor = false
        Instance.new("UICorner", ToggleButton).CornerRadius = UDim.new(1, 0)
        
        local ToggleIndicator = Instance.new("Frame")
        ToggleIndicator.Size = UDim2.new(0, 18, 0, 18)
        ToggleIndicator.Position = (Config[category] and Config[category][name]) and UDim2.new(1, -20, 0.5, -9) or UDim2.new(0, 2, 0.5, -9)
        ToggleIndicator.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        ToggleIndicator.BorderSizePixel = 0
        Instance.new("UICorner", ToggleIndicator).CornerRadius = UDim.new(1, 0)
        ToggleIndicator.Parent = ToggleButton
        ToggleButton.Parent = ToggleFrame
        
        ToggleButton.MouseButton1Click:Connect(function()
            local isEnabled = Config[category] and Config[category][name]
            if Config[category] then Config[category][name] = not isEnabled end
            if not isEnabled then
                TweenService:Create(ToggleButton, TweenInfo.new(0.2), {BackgroundColor3 = ItachiHub.Theme.Primary}):Play()
                TweenService:Create(ToggleIndicator, TweenInfo.new(0.2), {Position = UDim2.new(1, -20, 0.5, -9)}):Play()
            else
                TweenService:Create(ToggleButton, TweenInfo.new(0.2), {BackgroundColor3 = ItachiHub.Theme.SurfaceLight}):Play()
                TweenService:Create(ToggleIndicator, TweenInfo.new(0.2), {Position = UDim2.new(0, 2, 0.5, -9)}):Play()
            end
        end)
        ToggleFrame.Parent = parent
        return ToggleFrame
    end

    local function CreateSlider(category, name, min, max, default, parent, yPos)
        local SliderFrame = Instance.new("Frame")
        SliderFrame.Size = UDim2.new(1, -10, 0, 50)
        SliderFrame.Position = UDim2.new(0, 5, 0, yPos)
        SliderFrame.BackgroundColor3 = ItachiHub.Theme.Surface
        SliderFrame.BackgroundTransparency = 0.5
        SliderFrame.BorderSizePixel = 0
        Instance.new("UICorner", SliderFrame).CornerRadius = UDim.new(0, 6)
        
        local SliderLabel = Instance.new("TextLabel")
        SliderLabel.Size = UDim2.new(1, -20, 0, 16)
        SliderLabel.Position = UDim2.new(0, 8, 0, 3)
        SliderLabel.BackgroundTransparency = 1
        SliderLabel.Text = name .. ": " .. default
        SliderLabel.TextColor3 = ItachiHub.Theme.Text
        SliderLabel.TextSize = 11
        SliderLabel.Font = Enum.Font.Gotham
        SliderLabel.TextXAlignment = Enum.TextXAlignment.Left
        SliderLabel.Parent = SliderFrame
        
        local SliderBar = Instance.new("Frame")
        SliderBar.Size = UDim2.new(1, -30, 0, 4)
        SliderBar.Position = UDim2.new(0, 15, 0, 28)
        SliderBar.BackgroundColor3 = ItachiHub.Theme.SurfaceLight
        SliderBar.BorderSizePixel = 0
        SliderBar.Parent = SliderFrame
        
        local SliderFill = Instance.new("Frame")
        SliderFill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
        SliderFill.BackgroundColor3 = ItachiHub.Theme.Primary
        SliderFill.BorderSizePixel = 0
        SliderFill.Parent = SliderBar
        
        local SliderButton = Instance.new("TextButton")
        SliderButton.Size = UDim2.new(0, 14, 0, 14)
        SliderButton.Position = UDim2.new((default - min) / (max - min), -7, 0, -5)
        SliderButton.BackgroundColor3 = ItachiHub.Theme.Primary
        SliderButton.Text = ""
        SliderButton.BorderSizePixel = 0
        SliderButton.AutoButtonColor = false
        Instance.new("UICorner", SliderButton).CornerRadius = UDim.new(1, 0)
        SliderButton.Parent = SliderBar
        SliderFrame.Parent = parent
        
        local value = default
        local dragConnection
        SliderButton.MouseButton1Down:Connect(function()
            dragConnection = RunService.RenderStepped:Connect(function()
                if SliderBar.AbsoluteSize.X <= 0 then return end
                local percent = math.clamp((UserInputService:GetMouseLocation().X - SliderBar.AbsolutePosition.X) / SliderBar.AbsoluteSize.X, 0, 1)
                value = math.round(min + (max - min) * percent)
                if Config[category] then Config[category][name] = value end
                SliderFill.Size = UDim2.new(percent, 0, 1, 0)
                SliderButton.Position = UDim2.new(percent, -7, 0, -5)
                SliderLabel.Text = name .. ": " .. value
            end)
        end)
        UserInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 and dragConnection then dragConnection:Disconnect(); dragConnection = nil end
        end)
        return SliderFrame
    end

    -- Farm Tab
    local farmY = 0
    CreateToggle("Farm", "AutoFarm", TabContents["Farm"], farmY); farmY += 38
    CreateToggle("Farm", "AutoQuest", TabContents["Farm"], farmY); farmY += 38
    CreateToggle("Farm", "FastAttack", TabContents["Farm"], farmY); farmY += 38
    CreateToggle("Farm", "AutoBoss", TabContents["Farm"], farmY); farmY += 38
    CreateToggle("Farm", "AutoStats", TabContents["Farm"], farmY); farmY += 38
    CreateToggle("Farm", "AutoEquipBestSword", TabContents["Farm"], farmY); farmY += 38
    CreateToggle("Farm", "AutoCollectFruits", TabContents["Farm"], farmY)
    TabContents["Farm"].CanvasSize = UDim2.new(0, 0, 0, farmY + 40)

    -- Combat Tab
    local combatY = 0
    CreateToggle("Combat", "AutoClick", TabContents["Combat"], combatY); combatY += 38
    CreateSlider("Combat", "CPS", 1, 15, Config.Combat.CPS, TabContents["Combat"], combatY); combatY += 53
    CreateToggle("Combat", "AutoHaki", TabContents["Combat"], combatY); combatY += 38
    CreateToggle("Combat", "AutoBusoHaki", TabContents["Combat"], combatY); combatY += 38
    CreateToggle("Combat", "AutoKenHaki", TabContents["Combat"], combatY)
    TabContents["Combat"].CanvasSize = UDim2.new(0, 0, 0, combatY + 40)

    -- PvP Tab
    local pvpY = 0
    CreateSlider("PvP", "AimFOV", 50, 500, Config.PvP.AimFOV, TabContents["PvP"], pvpY); pvpY += 53
    CreateToggle("PvP", "ShowFOV", TabContents["PvP"], pvpY); pvpY += 38
    CreateToggle("PvP", "Aimbot", TabContents["PvP"], pvpY)
    TabContents["PvP"].CanvasSize = UDim2.new(0, 0, 0, pvpY + 40)

    -- Player Tab
    local playerY = 0
    CreateSlider("Player", "WalkSpeed", 16, 200, Config.Player.WalkSpeed, TabContents["Player"], playerY); playerY += 53
    CreateSlider("Player", "JumpPower", 50, 200, Config.Player.JumpPower, TabContents["Player"], playerY); playerY += 53
    CreateToggle("Player", "InfiniteJump", TabContents["Player"], playerY); playerY += 38
    CreateToggle("Player", "Fly", TabContents["Player"], playerY); playerY += 38
    CreateToggle("Player", "GodMode", TabContents["Player"], playerY)
    TabContents["Player"].CanvasSize = UDim2.new(0, 0, 0, playerY + 40)

    -- Visual Tab
    local visualY = 0
    CreateToggle("Visual", "ESP", TabContents["Visual"], visualY); visualY += 38
    CreateToggle("Visual", "FullBright", TabContents["Visual"], visualY); visualY += 38
    CreateToggle("Visual", "NoClip", TabContents["Visual"], visualY)
    TabContents["Visual"].CanvasSize = UDim2.new(0, 0, 0, visualY + 40)

    -- Misc Tab
    local miscY = 0
    CreateToggle("Misc", "AntiAFK", TabContents["Misc"], miscY); miscY += 38
    CreateToggle("Misc", "FPSBoost", TabContents["Misc"], miscY); miscY += 38
    CreateToggle("Misc", "AutoRedeemCodes", TabContents["Misc"], miscY); miscY += 38
    
    local RejoinButton = Instance.new("TextButton")
    RejoinButton.Size = UDim2.new(1, -10, 0, 30)
    RejoinButton.Position = UDim2.new(0, 5, 0, miscY)
    RejoinButton.BackgroundColor3 = ItachiHub.Theme.Primary
    RejoinButton.BackgroundTransparency = 0.3
    RejoinButton.Text = "🔄 REJOIN SERVER"
    RejoinButton.TextColor3 = ItachiHub.Theme.Text
    RejoinButton.TextSize = 12
    RejoinButton.Font = Enum.Font.GothamBold
    RejoinButton.BorderSizePixel = 0
    Instance.new("UICorner", RejoinButton).CornerRadius = UDim.new(0, 6)
    RejoinButton.MouseButton1Click:Connect(function() TeleportService:Teleport(game.PlaceId) end)
    RejoinButton.Parent = TabContents["Misc"]
    miscY += 40
    TabContents["Misc"].CanvasSize = UDim2.new(0, 0, 0, miscY + 40)

    -- Draggable
    local dragging, dragInput, dragStart, startPos
    TitleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = true; dragStart = input.Position; startPos = MainFrame.Position end
    end)
    TitleBar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then dragInput = input end
    end)
    RunService.RenderStepped:Connect(function()
        if dragging and dragInput then
            local delta = dragInput.Position - dragStart
            MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)

    return ScreenGui
end

-- NOTIFY FUNCTION (CRIADA ANTES DE USAR)
local ScreenGui
local function Notify(title, message, duration)
    pcall(function()
        if not ScreenGui then return end
        local notification = Instance.new("Frame")
        notification.Size = UDim2.new(0, 280, 0, 70)
        notification.Position = UDim2.new(1, 300, 0, 20)
        notification.BackgroundColor3 = ItachiHub.Theme.Surface
        notification.BackgroundTransparency = 0.2
        notification.BorderSizePixel = 0
        notification.Parent = ScreenGui
        Instance.new("UICorner", notification).CornerRadius = UDim.new(0, 8)
        local ns = Instance.new("UIStroke", notification); ns.Color = ItachiHub.Theme.Primary; ns.Thickness = 2; ns.Transparency = 0.5
        
        local notifTitle = Instance.new("TextLabel")
        notifTitle.Size = UDim2.new(1, -10, 0, 25); notifTitle.Position = UDim2.new(0, 5, 0, 5)
        notifTitle.BackgroundTransparency = 1; notifTitle.Text = title
        notifTitle.TextColor3 = ItachiHub.Theme.Primary; notifTitle.TextSize = 16
        notifTitle.Font = Enum.Font.GothamBold; notifTitle.TextXAlignment = Enum.TextXAlignment.Left
        notifTitle.Parent = notification
        
        local notifMsg = Instance.new("TextLabel")
        notifMsg.Size = UDim2.new(1, -10, 0, 30); notifMsg.Position = UDim2.new(0, 5, 0, 30)
        notifMsg.BackgroundTransparency = 1; notifMsg.Text = message
        notifMsg.TextColor3 = ItachiHub.Theme.Text; notifMsg.TextSize = 13
        notifMsg.Font = Enum.Font.Gotham; notifMsg.TextXAlignment = Enum.TextXAlignment.Left
        notifMsg.Parent = notification
        
        TweenService:Create(notification, TweenInfo.new(0.5, Enum.EasingStyle.Quart), {Position = UDim2.new(1, -300, 0, 20)}):Play()
        task.delay(duration or 5, function()
            pcall(function()
                TweenService:Create(notification, TweenInfo.new(0.5, Enum.EasingStyle.Quart), {Position = UDim2.new(1, 300, 0, 20)}):Play()
                task.wait(0.5); notification:Destroy()
            end)
        end)
    end)
end

-- Core Features
local function StartFeatures()
    table.insert(ItachiHub.Connections, RunService.Heartbeat:Connect(function()
        if Config.Farm.AutoFarm then
            pcall(function()
                local char = BF:GetCharacterSafe()
                if not char then return end
                local enemy = BF:GetNearestEnemy(300)
                if enemy and enemy:FindFirstChild("HumanoidRootPart") then
                    AntiDetectionCooldown()
                    char.HumanoidRootPart.CFrame = AntiBan:SafeCFrame(enemy.HumanoidRootPart.Position + Vector3.new(0, 5, 0))
                    BF:ClickSecure()
                end
            end)
        end
    end))

    table.insert(ItachiHub.Connections, RunService.Heartbeat:Connect(function()
        pcall(function()
            local char = BF:GetCharacterSafe()
            if not char then return end
            local humanoid = char:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = AntiBan:ValidateSpeed(Config.Player.WalkSpeed)
                humanoid.JumpPower = math.min(Config.Player.JumpPower, 200)
                if Config.Player.InfiniteJump then humanoid.Jump = true end
                if Config.Player.Fly and Config.Player.FlySpeed <= 100 then
                    humanoid.PlatformStand = true
                    char.HumanoidRootPart.CFrame = char.HumanoidRootPart.CFrame + Vector3.new(0, SecureRandom(0.3, 0.7), 0)
                end
            end
        end)
    end))

    table.insert(ItachiHub.Connections, RunService.Heartbeat:Connect(function()
        if Config.Misc.AntiAFK then
            pcall(function() game:GetService("VirtualUser"):CaptureController(); game:GetService("VirtualUser"):ClickButton2(Vector2.new()) end)
        end
    end))

    table.insert(ItachiHub.Connections, RunService.Heartbeat:Connect(function()
        if Config.Player.GodMode then
            pcall(function()
                local char = BF:GetCharacterSafe()
                if char then local humanoid = char:FindFirstChildOfClass("Humanoid"); if humanoid then humanoid.Health = humanoid.MaxHealth end end
            end)
        end
    end))

    table.insert(ItachiHub.Connections, RunService.Heartbeat:Connect(function()
        if Config.Visual.FullBright then
            pcall(function() Lighting.Brightness = 2; Lighting.ClockTime = 14; Lighting.FogEnd = 100000 end)
        end
    end))

    table.insert(ItachiHub.Connections, RunService.Heartbeat:Connect(function()
        if Config.Misc.FPSBoost then pcall(function() settings().Rendering.QualityLevel = 1 end) end
    end))
end

-- INITIALIZE
ScreenGui = CreateGUI()
StartFeatures()
LoadConfig()
Notify("🛡️ ITACHI HUB SECURE", "Version " .. ScriptVersion .. " | Anti-Ban Active", 6)

local function Cleanup()
    pcall(function()
        if ScreenGui then ScreenGui:Destroy() end
        for _, conn in ipairs(ItachiHub.Connections) do if conn then conn:Disconnect() end end
    end)
end

return {Config = Config, Cleanup = Cleanup, SaveConfig = SaveConfig, LoadConfig = LoadConfig, Notify = Notify}

end)

if not success then warn("❌ ItachiHub: Script error - " .. tostring(err)) end
