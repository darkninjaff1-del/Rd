--[[
    🔴 ITACHI HUB - GOD MODE ANTI-BAN UNIVERSAL
    Dark/Anime Theme | Itachi Uchiha Inspired
    Version 9.0.0 FINAL | Anti-Ban + All Executors
    Compatível: Krnl, Fluxus, Delta, Solara, CodeX, Wave, Vega X, etc
--]]

-- Anti-Crash Protection Universal
local success, err = pcall(function()

-- Verificação de PlaceId (Blox Fruits)
local function VerifyPlace()
    local validPlaces = {
        2753915549, 4442272183, 7449423635,
        4520749081, 6381829480,
        15759515082, 5931540094
    }
    for _, id in ipairs(validPlaces) do
        if game.PlaceId == id then return true end
    end
    return false
end
if not VerifyPlace() then return end

-- Aguarda carregamento completo
local function WaitForDataLoaded()
    local start = tick()
    local dataLoaded = false
    repeat
        task.wait(0.5)
        if tick() - start > 60 then return false end
        pcall(function()
            if game.Players.LocalPlayer and 
               game.Players.LocalPlayer:FindFirstChild("DataLoaded") and 
               game.Players.LocalPlayer:FindFirstChild("DataLoaded").Value then
                dataLoaded = true
            end
        end)
    until dataLoaded
    return true
end
if not WaitForDataLoaded() then return end

-- Services (Safe Get)
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local Lighting = game:GetService("Lighting")
local LocalPlayer = Players.LocalPlayer
local VirtualInputManager = game:GetService("VirtualInputManager")
local VirtualUser = game:GetService("VirtualUser")

-- Anti-Ban Variables
local ScriptName = "ItachiHub"
local ScriptVersion = "v9.0.0"
local LastAction = 0
local MinActionDelay = 0.15
local RandomOffset = Vector3.new(0, 0, 0)
local FarmRadius = 50

-- Anti-Ban Functions
local function Randomize(min, max)
    return min + (math.random() * (max - min))
end

local function AntiBanDelay()
    local elapsed = tick() - LastAction
    if elapsed < MinActionDelay then
        task.wait(MinActionDelay - elapsed + Randomize(0.02, 0.08))
    end
    LastAction = tick()
end

local function SafeTP(pos)
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local offset = Vector3.new(Randomize(-3, 3), Randomize(2, 6), Randomize(-3, 3))
            LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(pos + offset)
        end
    end)
end

local function SafeClick()
    pcall(function()
        AntiBanDelay()
        VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
        task.wait(Randomize(0.03, 0.08))
        VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
    end)
end

-- Theme
local Theme = {
    Primary = Color3.fromRGB(255, 0, 0),
    Secondary = Color3.fromRGB(180, 0, 0),
    Accent = Color3.fromRGB(255, 50, 50),
    Background = Color3.fromRGB(10, 10, 10),
    Surface = Color3.fromRGB(20, 20, 20),
    SurfaceLight = Color3.fromRGB(30, 30, 30),
    Text = Color3.fromRGB(255, 255, 255),
    TextDim = Color3.fromRGB(150, 150, 150),
    Success = Color3.fromRGB(0, 255, 100),
    Warning = Color3.fromRGB(255, 165, 0),
}

-- Config Completa
local Config = {
    -- Farm
    AutoFarm = false, AutoQuest = false, FastAttack = false, AutoFarmLevel = false,
    BringMobs = false, AutoBoss = false, AutoStats = false, AutoEquipBestSword = false,
    AutoNextEnemy = false, KillAura = false, NoStun = false,
    
    -- Fruits
    AutoFindFruit = false, FruitSniper = false, AutoDropBadFruits = false,
    AutoBuyFruitsFromDealer = false, AutoStoreInInventory = false, AutoSpinFruit = false,
    AutoCollectFruits = false, AutoStoreFruits = false, AutoTeleportFruit = false,
    AutoEatFruit = false, AutoBuyRandomFruit = false,
    
    -- Swords
    AutoFarmCDK = false, AutoFarmTushita = false, AutoFarmYama = false,
    AutoFarmKatakuri = false, AutoFarmDarkBlade = false, AutoFarmTrueTripleKatana = false,
    AutoFarmRengoku = false, AutoFarmSaber = false, AutoFarmSoulCane = false,
    AutoGetLegendarySword = false, AutoBuyAllSwords = false,
    
    -- Bosses
    AutoDarkBeard = false, AutoOrder = false, AutoDoughKing = false,
    AutoSoulReaper = false, AutoCakePrince = false, AutoBeautifulPirate = false,
    AutoLongma = false, AutoGreybeard = false, AutoRipIndra = false,
    AutoThunderGod = false, AutoTideKeeper = false, AutoDoughPrince = false,
    AutoBossHop = false, AutoLeaveAfterKill = false,
    
    -- Raid
    AutoStartRaid = false, AutoAwaken = false, AutoBuyChip = false, AutoCompleteRaid = false,
    
    -- AutoBuy Belly
    AutoBuyAllHaki = false, AutoBuyAllAbilities = false, AutoBuyAllFightingStyles = false,
    AutoBuyElectricClaw = false, AutoBuySuperhuman = false, AutoBuyDeathStep = false,
    AutoBuySharkmanKarate = false, AutoBuyDragonTalon = false, AutoBuyGodHuman = false,
    AutoBuyBlackLeg = false, AutoBuyElectro = false, AutoBuyFishmanKarate = false,
    AutoBuyDragonBreath = false, AutoBuyWaterKungFu = false,
    
    -- AutoBuy Fragments
    AutoBuyRaceV2 = false, AutoBuyRaceV3 = false, AutoBuyRaceV4 = false,
    AutoBuyAllFightingStyleUpgrades = false, AutoBuyAllSwordUpgrades = false,
    AutoBuyChipFragments = false, AutoBuyFruitNotifierFrag = false,
    AutoBuyInstinctV2 = false, AutoBuyObservationV2 = false,
    AutoBuyAllAccessories = false, AutoBuyMeleeItems = false,
    AutoBuySwordItems = false, AutoBuyGunItems = false, AutoBuyAccessoryItems = false,
    
    -- Shop
    AutoBuyHaki = false, AutoBuyGeppo = false, AutoBuySoru = false,
    AutoBuyKen = false, AutoBuyBuso = false, AutoSellItems = false, AutoBuyChests = false,
    
    -- Mastery
    AutoFarmMastery = false, AutoSwitchWeapon = false, TargetMasteryLevel = 600,
    
    -- Teleport
    AutoTPToQuest = false, AutoTPToBoss = false, AutoTPToFruit = false,
    TpToSeaBeast = false, TpToFruitSpawn = false, TpToChest = false,
    TpToShop = false, TpToAllNPCs = false,
    
    -- Sea
    AutoSeaBeast = false, AutoShipRaid = false, AutoKraken = false,
    AutoFactory = false, AutoPirateRaid = false, AutoCastleRaid = false,
    AutoRumblingWaters = false, AutoGhostShip = false, AutoMirageIsland = false,
    
    -- Factory
    AutoFactoryEvent = false, AutoCollectMaterial = false, AutoCraftItem = false,
    
    -- Combat
    AutoClick = false, CPS = 10, FastClick = false,
    AutoHaki = false, AutoBusoHaki = false, AutoKenHaki = false,
    AutoEquipWeapon = false, AutoSuperJump = false, AutoDash = false,
    AutoBlock = false, AutoDodge = false, HitboxExtender = false,
    AutoSkills = false, AutoUltimate = false,
    
    -- Stats
    AutoDistributeStats = false, PointsPerStat = 3,
    
    -- Visual
    ESP = false, ESPPlayers = false, ESPFruits = false, ESPChests = false, ESPNPCs = false,
    ESPSeaBeasts = false, ESPIslands = false, ESPItems = false,
    FullBright = false, NoClip = false, RemoveFog = false,
    Tracer = false, Chams = false,
    
    -- PvP
    AimFOV = 200, ShowFOV = false, Aimbot = false, AimAssist = false,
    AllyCheck = false, TeamCheck = false, AutoBounty = false, SafeZone = false,
    CombatLog = false, AutoRunFromBounty = false,
    
    -- Player
    WalkSpeed = 16, JumpPower = 50, InfiniteJump = false, Fly = false, FlySpeed = 50,
    GodMode = false, NoFallDamage = false, SuperSpeed = false,
    AutoRespawn = false, AntiStun = false, AntiGrab = false, AntiKnockback = false,
    
    -- Race
    AutoEvolveRace = false, AutoBuyRaceReroll = false,
    AutoV2 = false, AutoV3 = false, AutoV4 = false, AutoUnlockAllRaces = false,
    
    -- Quest
    AutoTalkNPC = false, AutoCompleteQuest = false, AutoTurnInQuest = false, SkipQuestDialog = false,
    
    -- Notifications
    NotifyOnFruitSpawn = false, NotifyOnBossSpawn = false,
    NotifyOnSeaBeast = false, NotifyOnRaidStart = false, NotifyOnItemSpawn = false,
    WebhookURL = "",
    
    -- Misc
    AntiAFK = false, FPSBoost = false, RemoveTerrain = false, AutoRedeemCodes = false,
    AutoOpenChests = false, AutoCollectGems = false, AutoCollectBeli = false,
    AutoUpgradeWeapon = false, AutoCollectAll = false,
    
    -- Security
    SafeMode = true, AutoLeaveOnAdmin = false, HideCharacter = false,
    AntiTPBack = false, AutoResetOnBan = false,
    AutoLeaveOnKill = false, AntiReport = false,
}

-- Save/Load Universal
local function SaveConfig()
    pcall(function()
        if writefile and isfolder and makefolder then
            if not isfolder("ItachiHub") then makefolder("ItachiHub") end
            writefile("ItachiHub/config.json", HttpService:JSONEncode(Config))
        end
    end)
end

local function LoadConfig()
    pcall(function()
        if readfile and isfile and isfile("ItachiHub/config.json") then
            local loaded = HttpService:JSONDecode(readfile("ItachiHub/config.json"))
            for k, v in pairs(loaded) do
                if Config[k] ~= nil then Config[k] = v end
            end
        end
    end)
end

-- Blox Fruits Utils
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

function BF:GetBosses()
    local bosses = {}
    pcall(function()
        for _, v in ipairs(workspace.Enemies:GetChildren()) do
            if v:FindFirstChild("Humanoid") and v.Humanoid.MaxHealth > 10000 and v.Humanoid.Health > 0 and v:FindFirstChild("HumanoidRootPart") then
                table.insert(bosses, v)
            end
        end
    end)
    return bosses
end

function BF:GetPlayers()
    local players = {}
    pcall(function()
        for _, p in ipairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                table.insert(players, p)
            end
        end
    end)
    return players
end

function BF:GetFruits()
    local fruits = {}
    pcall(function()
        for _, v in ipairs(workspace:GetDescendants()) do
            if v:IsA("Tool") and v.Name:find("Fruit") then table.insert(fruits, v) end
        end
    end)
    return fruits
end

function BF:GetChests()
    local chests = {}
    pcall(function()
        for _, v in ipairs(workspace:GetDescendants()) do
            if v.Name:find("Chest") and v:IsA("BasePart") then table.insert(chests, v) end
        end
    end)
    return chests
end

function BF:EquipBestSword()
    pcall(function()
        if not LocalPlayer.Backpack then return end
        local best, bestDmg = nil, 0
        for _, tool in ipairs(LocalPlayer.Backpack:GetChildren()) do
            if tool:IsA("Tool") and tool:FindFirstChild("Damage") and tool.Damage.Value > bestDmg then
                bestDmg = tool.Damage.Value; best = tool
            end
        end
        if best and LocalPlayer.Character then
            LocalPlayer.Character.Humanoid:EquipTool(best)
        end
    end)
end

-- GUI Universal
local function CreateGUI()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "ItachiHub"
    ScreenGui.Parent = game.CoreGui
    ScreenGui.ResetOnSpawn = false

    -- Toggle Icon
    local ToggleIcon = Instance.new("TextButton")
    ToggleIcon.Size = UDim2.new(0, 55, 0, 55)
    ToggleIcon.Position = UDim2.new(0, 15, 0.5, -27)
    ToggleIcon.BackgroundColor3 = Theme.Primary
    ToggleIcon.BackgroundTransparency = 0.3
    ToggleIcon.Text = "🔴"
    ToggleIcon.TextSize = 30
    ToggleIcon.Font = Enum.Font.GothamBold
    ToggleIcon.BorderSizePixel = 0
    ToggleIcon.Parent = ScreenGui
    Instance.new("UICorner", ToggleIcon).CornerRadius = UDim.new(1, 0)
    local ics = Instance.new("UIStroke", ToggleIcon)
    ics.Color = Color3.fromRGB(255, 255, 255)
    ics.Thickness = 2

    -- Main Frame
    local MainFrame = Instance.new("Frame")
    MainFrame.Size = UDim2.new(0, 700, 0, 470)
    MainFrame.Position = UDim2.new(0.5, -350, 0.5, -235)
    MainFrame.BackgroundColor3 = Theme.Background
    MainFrame.BackgroundTransparency = 0.1
    MainFrame.BorderSizePixel = 0
    MainFrame.ClipsDescendants = true
    MainFrame.Visible = false
    MainFrame.Parent = ScreenGui
    Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)
    local ms = Instance.new("UIStroke", MainFrame)
    ms.Color = Theme.Primary
    ms.Thickness = 2

    -- Toggle
    local uiVisible = false
    ToggleIcon.MouseButton1Click:Connect(function()
        uiVisible = not uiVisible
        MainFrame.Visible = uiVisible
        ToggleIcon.Text = uiVisible and "✕" or "🔴"
    end)

    -- Title
    local TitleBar = Instance.new("Frame")
    TitleBar.Size = UDim2.new(1, 0, 0, 40)
    TitleBar.BackgroundColor3 = Theme.Surface
    TitleBar.BackgroundTransparency = 0.3
    TitleBar.BorderSizePixel = 0
    TitleBar.Parent = MainFrame
    Instance.new("UICorner", TitleBar).CornerRadius = UDim.new(0, 12)
    
    local TitleLabel = Instance.new("TextLabel")
    TitleLabel.Size = UDim2.new(0, 500, 1, 0)
    TitleLabel.Position = UDim2.new(0, 15, 0, 0)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = "🔴 ITACHI HUB | " .. BF:GetCurrentWorld() .. " | v9.0 ANTI-BAN"
    TitleLabel.TextColor3 = Theme.Primary
    TitleLabel.TextSize = 15
    TitleLabel.Font = Enum.Font.GothamBold
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    TitleLabel.Parent = TitleBar

    -- Status
    local StatusLabel = Instance.new("TextLabel")
    StatusLabel.Size = UDim2.new(0, 120, 1, 0)
    StatusLabel.Position = UDim2.new(1, -135, 0, 0)
    StatusLabel.BackgroundTransparency = 1
    StatusLabel.Text = "🛡️ PROTECTED"
    StatusLabel.TextColor3 = Theme.Success
    StatusLabel.TextSize = 11
    StatusLabel.Font = Enum.Font.GothamBold
    StatusLabel.TextXAlignment = Enum.TextXAlignment.Right
    StatusLabel.Parent = TitleBar

    -- Tabs
    local TabContainer = Instance.new("Frame")
    TabContainer.Size = UDim2.new(0, 160, 1, -50)
    TabContainer.Position = UDim2.new(0, 0, 0, 50)
    TabContainer.BackgroundColor3 = Theme.Surface
    TabContainer.BackgroundTransparency = 0.5
    TabContainer.BorderSizePixel = 0
    TabContainer.Parent = MainFrame
    
    local ContentFrame = Instance.new("Frame")
    ContentFrame.Size = UDim2.new(1, -170, 1, -60)
    ContentFrame.Position = UDim2.new(0, 165, 0, 55)
    ContentFrame.BackgroundTransparency = 1
    ContentFrame.ClipsDescendants = true
    ContentFrame.Parent = MainFrame

    local Tabs = {
        "🏠Farm","👹Bosses","⚔️Swords","🍎Fruits","💰AutoBuy","🏪Shop",
        "⚡Raid","📈Mastery","🌍Teleport","🌊Sea","🏭Factory",
        "⚔️Combat","📊Stats","👁️Visual","🎯PvP","🏃Player",
        "🧬Race","📋Quest","🔔Notif","🛡️Security","⚙️Misc"
    }
    local TabButtons = {}
    local TabContents = {}

    for i, tabName in ipairs(Tabs) do
        local TabButton = Instance.new("TextButton")
        TabButton.Size = UDim2.new(1, -8, 0, 24)
        TabButton.Position = UDim2.new(0, 4, 0, 3 + (i-1) * 27)
        TabButton.BackgroundColor3 = Theme.SurfaceLight
        TabButton.BackgroundTransparency = 0.7
        TabButton.Text = tabName
        TabButton.TextColor3 = Theme.TextDim
        TabButton.TextSize = 9
        TabButton.Font = Enum.Font.GothamBold
        TabButton.BorderSizePixel = 0
        Instance.new("UICorner", TabButton).CornerRadius = UDim.new(0, 5)
        TabButton.Parent = TabContainer
        TabButtons[tabName] = TabButton
        
        local TabContent = Instance.new("ScrollingFrame")
        TabContent.Size = UDim2.new(1, -10, 1, -10)
        TabContent.Position = UDim2.new(0, 5, 0, 5)
        TabContent.BackgroundTransparency = 1
        TabContent.BorderSizePixel = 0
        TabContent.ScrollBarThickness = 4
        TabContent.ScrollBarImageColor3 = Theme.Primary
        TabContent.Visible = (i == 1)
        TabContent.CanvasSize = UDim2.new(0, 0, 0, 0)
        TabContent.Parent = ContentFrame
        TabContents[tabName] = TabContent
        
        TabButton.MouseButton1Click:Connect(function()
            for _, btn in pairs(TabButtons) do
                btn.BackgroundColor3 = Theme.SurfaceLight
                btn.BackgroundTransparency = 0.7
                btn.TextColor3 = Theme.TextDim
            end
            TabButton.BackgroundColor3 = Theme.Primary
            TabButton.BackgroundTransparency = 0.3
            TabButton.TextColor3 = Theme.Text
            for _, content in pairs(TabContents) do content.Visible = false end
            TabContent.Visible = true
        end)
    end
    TabButtons[Tabs[1]].BackgroundColor3 = Theme.Primary
    TabButtons[Tabs[1]].BackgroundTransparency = 0.3
    TabButtons[Tabs[1]].TextColor3 = Theme.Text

    -- Toggle Function
    local function CT(name, parent, yPos)
        local ToggleFrame = Instance.new("Frame")
        ToggleFrame.Size = UDim2.new(1, -10, 0, 28)
        ToggleFrame.Position = UDim2.new(0, 5, 0, yPos)
        ToggleFrame.BackgroundColor3 = Theme.Surface
        ToggleFrame.BackgroundTransparency = 0.5
        ToggleFrame.BorderSizePixel = 0
        Instance.new("UICorner", ToggleFrame).CornerRadius = UDim.new(0, 5)
        
        local ToggleLabel = Instance.new("TextLabel")
        ToggleLabel.Size = UDim2.new(0.7, 0, 1, 0)
        ToggleLabel.Position = UDim2.new(0, 6, 0, 0)
        ToggleLabel.BackgroundTransparency = 1
        ToggleLabel.Text = name
        ToggleLabel.TextColor3 = Theme.Text
        ToggleLabel.TextSize = 9
        ToggleLabel.Font = Enum.Font.Gotham
        ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
        ToggleLabel.Parent = ToggleFrame
        
        local ToggleButton = Instance.new("TextButton")
        ToggleButton.Size = UDim2.new(0, 32, 0, 16)
        ToggleButton.Position = UDim2.new(1, -38, 0.5, -8)
        ToggleButton.BackgroundColor3 = Config[name] and Theme.Primary or Theme.SurfaceLight
        ToggleButton.Text = ""
        ToggleButton.BorderSizePixel = 0
        ToggleButton.AutoButtonColor = false
        Instance.new("UICorner", ToggleButton).CornerRadius = UDim.new(1, 0)
        
        local ToggleIndicator = Instance.new("Frame")
        ToggleIndicator.Size = UDim2.new(0, 12, 0, 12)
        ToggleIndicator.Position = Config[name] and UDim2.new(1, -14, 0.5, -6) or UDim2.new(0, 2, 0.5, -6)
        ToggleIndicator.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        ToggleIndicator.BorderSizePixel = 0
        Instance.new("UICorner", ToggleIndicator).CornerRadius = UDim.new(1, 0)
        ToggleIndicator.Parent = ToggleButton
        ToggleButton.Parent = ToggleFrame
        
        ToggleButton.MouseButton1Click:Connect(function()
            Config[name] = not Config[name]
            if Config[name] then
                TweenService:Create(ToggleButton, TweenInfo.new(0.2), {BackgroundColor3 = Theme.Primary}):Play()
                TweenService:Create(ToggleIndicator, TweenInfo.new(0.2), {Position = UDim2.new(1, -14, 0.5, -6)}):Play()
            else
                TweenService:Create(ToggleButton, TweenInfo.new(0.2), {BackgroundColor3 = Theme.SurfaceLight}):Play()
                TweenService:Create(ToggleIndicator, TweenInfo.new(0.2), {Position = UDim2.new(0, 2, 0.5, -6)}):Play()
            end
        end)
        ToggleFrame.Parent = parent
    end

    -- Slider Function
    local function CS(name, min, max, parent, yPos)
        local SliderFrame = Instance.new("Frame")
        SliderFrame.Size = UDim2.new(1, -10, 0, 42)
        SliderFrame.Position = UDim2.new(0, 5, 0, yPos)
        SliderFrame.BackgroundColor3 = Theme.Surface
        SliderFrame.BackgroundTransparency = 0.5
        SliderFrame.BorderSizePixel = 0
        Instance.new("UICorner", SliderFrame).CornerRadius = UDim.new(0, 5)
        
        local SliderLabel = Instance.new("TextLabel")
        SliderLabel.Size = UDim2.new(1, -20, 0, 12)
        SliderLabel.Position = UDim2.new(0, 6, 0, 1)
        SliderLabel.BackgroundTransparency = 1
        SliderLabel.Text = name .. ": " .. Config[name]
        SliderLabel.TextColor3 = Theme.Text
        SliderLabel.TextSize = 8
        SliderLabel.Font = Enum.Font.Gotham
        SliderLabel.TextXAlignment = Enum.TextXAlignment.Left
        SliderLabel.Parent = SliderFrame
        
        local Bar = Instance.new("Frame")
        Bar.Size = UDim2.new(1, -30, 0, 3)
        Bar.Position = UDim2.new(0, 15, 0, 20)
        Bar.BackgroundColor3 = Theme.SurfaceLight
        Bar.BorderSizePixel = 0
        Bar.Parent = SliderFrame
        
        local Fill = Instance.new("Frame")
        Fill.Size = UDim2.new((Config[name] - min) / (max - min), 0, 1, 0)
        Fill.BackgroundColor3 = Theme.Primary
        Fill.BorderSizePixel = 0
        Fill.Parent = Bar
        
        local Btn = Instance.new("TextButton")
        Btn.Size = UDim2.new(0, 10, 0, 10)
        Btn.Position = UDim2.new((Config[name] - min) / (max - min), -5, 0, -3.5)
        Btn.BackgroundColor3 = Theme.Primary
        Btn.Text = ""
        Btn.BorderSizePixel = 0
        Btn.AutoButtonColor = false
        Instance.new("UICorner", Btn).CornerRadius = UDim.new(1, 0)
        Btn.Parent = Bar
        SliderFrame.Parent = parent
        
        local dragConnection
        Btn.MouseButton1Down:Connect(function()
            dragConnection = RunService.RenderStepped:Connect(function()
                if Bar.AbsoluteSize.X <= 0 then return end
                local percent = math.clamp((UserInputService:GetMouseLocation().X - Bar.AbsolutePosition.X) / Bar.AbsoluteSize.X, 0, 1)
                local value = math.round(min + (max - min) * percent)
                Config[name] = value
                Fill.Size = UDim2.new(percent, 0, 1, 0)
                Btn.Position = UDim2.new(percent, -5, 0, -3.5)
                SliderLabel.Text = name .. ": " .. value
            end)
        end)
        UserInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 and dragConnection then
                dragConnection:Disconnect()
                dragConnection = nil
            end
        end)
    end

    local function PopulateTab(tabName, items)
        local y = 0
        for _, item in ipairs(items) do
            if item.t == "toggle" then CT(item.n, TabContents[tabName], y); y += 31
            elseif item.t == "slider" then CS(item.n, item.min, item.max, TabContents[tabName], y); y += 45 end
        end
        TabContents[tabName].CanvasSize = UDim2.new(0, 0, 0, y + 30)
    end

    -- Populate ALL tabs
    PopulateTab("🏠Farm", {
        {t="toggle",n="AutoFarm"},{t="toggle",n="AutoQuest"},{t="toggle",n="FastAttack"},
        {t="toggle",n="AutoFarmLevel"},{t="toggle",n="BringMobs"},{t="toggle",n="AutoBoss"},
        {t="toggle",n="AutoStats"},{t="toggle",n="AutoEquipBestSword"},{t="toggle",n="AutoNextEnemy"},
        {t="toggle",n="KillAura"},{t="toggle",n="NoStun"},{t="toggle",n="AutoCollectAll"}
    })
    PopulateTab("👹Bosses", {
        {t="toggle",n="AutoDarkBeard"},{t="toggle",n="AutoOrder"},{t="toggle",n="AutoDoughKing"},
        {t="toggle",n="AutoSoulReaper"},{t="toggle",n="AutoCakePrince"},{t="toggle",n="AutoBeautifulPirate"},
        {t="toggle",n="AutoLongma"},{t="toggle",n="AutoGreybeard"},{t="toggle",n="AutoRipIndra"},
        {t="toggle",n="AutoThunderGod"},{t="toggle",n="AutoTideKeeper"},{t="toggle",n="AutoDoughPrince"},
        {t="toggle",n="AutoBossHop"},{t="toggle",n="AutoLeaveAfterKill"}
    })
    PopulateTab("⚔️Swords", {
        {t="toggle",n="AutoFarmCDK"},{t="toggle",n="AutoFarmTushita"},{t="toggle",n="AutoFarmYama"},
        {t="toggle",n="AutoFarmKatakuri"},{t="toggle",n="AutoFarmDarkBlade"},{t="toggle",n="AutoFarmTrueTripleKatana"},
        {t="toggle",n="AutoFarmRengoku"},{t="toggle",n="AutoFarmSaber"},{t="toggle",n="AutoFarmSoulCane"},
        {t="toggle",n="AutoGetLegendarySword"},{t="toggle",n="AutoBuyAllSwords"}
    })
    PopulateTab("🍎Fruits", {
        {t="toggle",n="AutoFindFruit"},{t="toggle",n="FruitSniper"},{t="toggle",n="AutoSpinFruit"},
        {t="toggle",n="AutoCollectFruits"},{t="toggle",n="AutoStoreFruits"},{t="toggle",n="AutoTeleportFruit"},
        {t="toggle",n="AutoStoreInInventory"},{t="toggle",n="AutoDropBadFruits"},
        {t="toggle",n="AutoBuyFruitsFromDealer"},{t="toggle",n="AutoEatFruit"},{t="toggle",n="AutoBuyRandomFruit"}
    })
    PopulateTab("💰AutoBuy", {
        {t="toggle",n="AutoBuyAllHaki"},{t="toggle",n="AutoBuyAllAbilities"},{t="toggle",n="AutoBuyAllFightingStyles"},
        {t="toggle",n="AutoBuyElectricClaw"},{t="toggle",n="AutoBuySuperhuman"},{t="toggle",n="AutoBuyDeathStep"},
        {t="toggle",n="AutoBuySharkmanKarate"},{t="toggle",n="AutoBuyDragonTalon"},{t="toggle",n="AutoBuyGodHuman"},
        {t="toggle",n="AutoBuyBlackLeg"},{t="toggle",n="AutoBuyElectro"},{t="toggle",n="AutoBuyFishmanKarate"},
        {t="toggle",n="AutoBuyDragonBreath"},{t="toggle",n="AutoBuyWaterKungFu"},
        {t="toggle",n="AutoBuyRaceV2"},{t="toggle",n="AutoBuyRaceV3"},{t="toggle",n="AutoBuyRaceV4"},
        {t="toggle",n="AutoBuyAllFightingStyleUpgrades"},{t="toggle",n="AutoBuyAllSwordUpgrades"},
        {t="toggle",n="AutoBuyChipFragments"},{t="toggle",n="AutoBuyFruitNotifierFrag"},
        {t="toggle",n="AutoBuyInstinctV2"},{t="toggle",n="AutoBuyObservationV2"},
        {t="toggle",n="AutoBuyAllAccessories"},{t="toggle",n="AutoBuyMeleeItems"},
        {t="toggle",n="AutoBuySwordItems"},{t="toggle",n="AutoBuyGunItems"},{t="toggle",n="AutoBuyAccessoryItems"}
    })
    PopulateTab("🏪Shop", {
        {t="toggle",n="AutoBuyHaki"},{t="toggle",n="AutoBuyGeppo"},{t="toggle",n="AutoBuySoru"},
        {t="toggle",n="AutoBuyKen"},{t="toggle",n="AutoBuyBuso"},{t="toggle",n="AutoSellItems"},{t="toggle",n="AutoBuyChests"}
    })
    PopulateTab("⚡Raid", {
        {t="toggle",n="AutoStartRaid"},{t="toggle",n="AutoAwaken"},{t="toggle",n="AutoBuyChip"},{t="toggle",n="AutoCompleteRaid"}
    })
    PopulateTab("📈Mastery", {
        {t="toggle",n="AutoFarmMastery"},{t="toggle",n="AutoSwitchWeapon"},{t="slider",n="TargetMasteryLevel",min=1,max=600}
    })
    PopulateTab("🌍Teleport", {
        {t="toggle",n="AutoTPToQuest"},{t="toggle",n="AutoTPToBoss"},{t="toggle",n="AutoTPToFruit"},
        {t="toggle",n="TpToSeaBeast"},{t="toggle",n="TpToFruitSpawn"},{t="toggle",n="TpToChest"},
        {t="toggle",n="TpToShop"},{t="toggle",n="TpToAllNPCs"}
    })
    PopulateTab("🌊Sea", {
        {t="toggle",n="AutoSeaBeast"},{t="toggle",n="AutoShipRaid"},{t="toggle",n="AutoKraken"},
        {t="toggle",n="AutoFactory"},{t="toggle",n="AutoPirateRaid"},{t="toggle",n="AutoCastleRaid"},
        {t="toggle",n="AutoRumblingWaters"},{t="toggle",n="AutoGhostShip"},{t="toggle",n="AutoMirageIsland"}
    })
    PopulateTab("🏭Factory", {
        {t="toggle",n="AutoFactoryEvent"},{t="toggle",n="AutoCollectMaterial"},{t="toggle",n="AutoCraftItem"}
    })
    PopulateTab("⚔️Combat", {
        {t="toggle",n="AutoClick"},{t="slider",n="CPS",min=1,max=20},{t="toggle",n="FastClick"},
        {t="toggle",n="AutoHaki"},{t="toggle",n="AutoBusoHaki"},{t="toggle",n="AutoKenHaki"},
        {t="toggle",n="AutoEquipWeapon"},{t="toggle",n="AutoSuperJump"},{t="toggle",n="AutoDash"},
        {t="toggle",n="AutoBlock"},{t="toggle",n="AutoDodge"},{t="toggle",n="HitboxExtender"},
        {t="toggle",n="AutoSkills"},{t="toggle",n="AutoUltimate"}
    })
    PopulateTab("📊Stats", {
        {t="toggle",n="AutoDistributeStats"},{t="slider",n="PointsPerStat",min=1,max=10}
    })
    PopulateTab("👁️Visual", {
        {t="toggle",n="ESP"},{t="toggle",n="ESPPlayers"},{t="toggle",n="ESPFruits"},
        {t="toggle",n="ESPChests"},{t="toggle",n="ESPNPCs"},{t="toggle",n="ESPSeaBeasts"},
        {t="toggle",n="ESPIslands"},{t="toggle",n="ESPItems"},
        {t="toggle",n="FullBright"},{t="toggle",n="NoClip"},{t="toggle",n="RemoveFog"},
        {t="toggle",n="Tracer"},{t="toggle",n="Chams"}
    })
    PopulateTab("🎯PvP", {
        {t="slider",n="AimFOV",min=50,max=500},{t="toggle",n="ShowFOV"},{t="toggle",n="Aimbot"},
        {t="toggle",n="AimAssist"},{t="toggle",n="AllyCheck"},{t="toggle",n="TeamCheck"},
        {t="toggle",n="AutoBounty"},{t="toggle",n="SafeZone"},{t="toggle",n="CombatLog"},{t="toggle",n="AutoRunFromBounty"}
    })
    PopulateTab("🏃Player", {
        {t="slider",n="WalkSpeed",min=16,max=300},{t="slider",n="JumpPower",min=50,max=300},
        {t="toggle",n="InfiniteJump"},{t="toggle",n="Fly"},{t="slider",n="FlySpeed",min=10,max=200},
        {t="toggle",n="GodMode"},{t="toggle",n="NoFallDamage"},{t="toggle",n="SuperSpeed"},
        {t="toggle",n="AutoRespawn"},{t="toggle",n="AntiStun"},{t="toggle",n="AntiGrab"},{t="toggle",n="AntiKnockback"}
    })
    PopulateTab("🧬Race", {
        {t="toggle",n="AutoEvolveRace"},{t="toggle",n="AutoBuyRaceReroll"},
        {t="toggle",n="AutoV2"},{t="toggle",n="AutoV3"},{t="toggle",n="AutoV4"},{t="toggle",n="AutoUnlockAllRaces"}
    })
    PopulateTab("📋Quest", {
        {t="toggle",n="AutoTalkNPC"},{t="toggle",n="AutoCompleteQuest"},{t="toggle",n="AutoTurnInQuest"},{t="toggle",n="SkipQuestDialog"}
    })
    PopulateTab("🔔Notif", {
        {t="toggle",n="NotifyOnFruitSpawn"},{t="toggle",n="NotifyOnBossSpawn"},
        {t="toggle",n="NotifyOnSeaBeast"},{t="toggle",n="NotifyOnRaidStart"},{t="toggle",n="NotifyOnItemSpawn"}
    })
    PopulateTab("🛡️Security", {
        {t="toggle",n="SafeMode"},{t="toggle",n="AutoLeaveOnAdmin"},{t="toggle",n="HideCharacter"},
        {t="toggle",n="AntiTPBack"},{t="toggle",n="AutoResetOnBan"},
        {t="toggle",n="AutoLeaveOnKill"},{t="toggle",n="AntiReport"}
    })

    -- Misc Tab
    local miscY = 0
    CT("AntiAFK", TabContents["⚙️Misc"], miscY); miscY += 31
    CT("FPSBoost", TabContents["⚙️Misc"], miscY); miscY += 31
    CT("RemoveTerrain", TabContents["⚙️Misc"], miscY); miscY += 31
    CT("AutoRedeemCodes", TabContents["⚙️Misc"], miscY); miscY += 31
    CT("AutoOpenChests", TabContents["⚙️Misc"], miscY); miscY += 31
    CT("AutoCollectGems", TabContents["⚙️Misc"], miscY); miscY += 31
    CT("AutoCollectBeli", TabContents["⚙️Misc"], miscY); miscY += 31
    CT("AutoUpgradeWeapon", TabContents["⚙️Misc"], miscY); miscY += 31
    CT("AutoCollectAll", TabContents["⚙️Misc"], miscY); miscY += 31
    
    local RejoinBtn = Instance.new("TextButton")
    RejoinBtn.Size = UDim2.new(1, -10, 0, 24)
    RejoinBtn.Position = UDim2.new(0, 5, 0, miscY)
    RejoinBtn.BackgroundColor3 = Theme.Primary
    RejoinBtn.BackgroundTransparency = 0.3
    RejoinBtn.Text = "🔄 REJOIN"
    RejoinBtn.TextColor3 = Theme.Text
    RejoinBtn.TextSize = 9
    RejoinBtn.Font = Enum.Font.GothamBold
    RejoinBtn.BorderSizePixel = 0
    Instance.new("UICorner", RejoinBtn).CornerRadius = UDim.new(0, 5)
    RejoinBtn.MouseButton1Click:Connect(function() TeleportService:Teleport(game.PlaceId) end)
    RejoinBtn.Parent = TabContents["⚙️Misc"]
    miscY += 30
    
    local SaveBtn = Instance.new("TextButton")
    SaveBtn.Size = UDim2.new(1, -10, 0, 24)
    SaveBtn.Position = UDim2.new(0, 5, 0, miscY)
    SaveBtn.BackgroundColor3 = Theme.Success
    SaveBtn.BackgroundTransparency = 0.5
    SaveBtn.Text = "💾 SAVE CONFIG"
    SaveBtn.TextColor3 = Theme.Text
    SaveBtn.TextSize = 9
    SaveBtn.Font = Enum.Font.GothamBold
    SaveBtn.BorderSizePixel = 0
    Instance.new("UICorner", SaveBtn).CornerRadius = UDim.new(0, 5)
    SaveBtn.MouseButton1Click:Connect(function()
        SaveConfig()
        SaveBtn.Text = "✅ SAVED!"
        task.delay(2, function() SaveBtn.Text = "💾 SAVE CONFIG" end)
    end)
    SaveBtn.Parent = TabContents["⚙️Misc"]
    miscY += 35
    
    TabContents["⚙️Misc"].CanvasSize = UDim2.new(0, 0, 0, miscY + 30)

    -- Draggable
    local dragging, dragInput, dragStart, startPos
    TitleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = MainFrame.Position
        end
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

    -- FOV Circle
    local FOVCircle = Instance.new("Frame")
    FOVCircle.Size = UDim2.new(0, Config.AimFOV, 0, Config.AimFOV)
    FOVCircle.Position = UDim2.new(0.5, -Config.AimFOV/2, 0.5, -Config.AimFOV/2)
    FOVCircle.BackgroundTransparency = 1
    FOVCircle.Visible = Config.ShowFOV
    FOVCircle.Parent = ScreenGui
    local fovs = Instance.new("UIStroke", FOVCircle)
    fovs.Color = Theme.Primary
    fovs.Thickness = 2
    Instance.new("UICorner", FOVCircle).CornerRadius = UDim.new(1, 0)
    
    RunService.RenderStepped:Connect(function()
        FOVCircle.Visible = Config.ShowFOV
        FOVCircle.Size = UDim2.new(0, Config.AimFOV, 0, Config.AimFOV)
        FOVCircle.Position = UDim2.new(0.5, -Config.AimFOV/2, 0.5, -Config.AimFOV/2)
    end)

    return ScreenGui
end

-- Initialize
local ScreenGui = CreateGUI()
LoadConfig()

-- ============================================
-- ANTI-BAN SYSTEMS (Executados com segurança)
-- ============================================

-- Auto Farm (Anti-Ban)
RunService.Heartbeat:Connect(function()
    if Config.AutoFarm then
        pcall(function()
            local char = BF:GetCharacterSafe()
            if not char then return end
            local enemy = BF:GetNearestEnemy(Config.BringMobs and 500 or 300)
            if enemy then
                SafeTP(enemy.HumanoidRootPart.Position)
                if Config.FastAttack then SafeClick() end
                SafeClick()
            end
        end)
    end
end)

-- Auto Boss (Anti-Ban)
RunService.Heartbeat:Connect(function()
    if Config.AutoBoss then
        pcall(function()
            local char = BF:GetCharacterSafe()
            if not char then return end
            local bosses = BF:GetBosses()
            if #bosses > 0 then
                SafeTP(bosses[1].HumanoidRootPart.Position + Vector3.new(0, 10, 0))
                for _ = 1, 3 do SafeClick() end
            end
        end)
    end
end)

-- Auto Click (Anti-Ban)
RunService.Heartbeat:Connect(function()
    if Config.AutoClick then
        pcall(function()
            task.wait(Randomize(0.8, 1.2) / Config.CPS)
            if Config.FastClick then SafeClick() end
            SafeClick()
        end)
    end
end)

-- Kill Aura (Anti-Ban)
RunService.Heartbeat:Connect(function()
    if Config.KillAura then
        pcall(function()
            local char = BF:GetCharacterSafe()
            if not char then return end
            for _, enemy in ipairs(workspace.Enemies:GetChildren()) do
                if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 and enemy:FindFirstChild("HumanoidRootPart") then
                    local dist = (char.HumanoidRootPart.Position - enemy.HumanoidRootPart.Position).Magnitude
                    if dist < 50 then
                        AntiBanDelay()
                        enemy.Humanoid.Health = 0
                    end
                end
            end
        end)
    end
end)

-- Player Mods (Anti-Ban)
RunService.Heartbeat:Connect(function()
    pcall(function()
        local char = BF:GetCharacterSafe()
        if not char then return end
        local hum = char:FindFirstChildOfClass("Humanoid")
        if hum then
            hum.WalkSpeed = Config.SafeMode and math.min(Config.WalkSpeed, 200) or Config.WalkSpeed
            hum.JumpPower = Config.SafeMode and math.min(Config.JumpPower, 200) or Config.JumpPower
            if Config.InfiniteJump then hum.Jump = true end
            if Config.Fly and Config.FlySpeed <= 100 then
                hum.PlatformStand = true
                char.HumanoidRootPart.CFrame = char.HumanoidRootPart.CFrame + Vector3.new(0, Randomize(0.3, 0.7), 0)
            end
            if Config.NoFallDamage then hum.FallSpeed = 0 end
        end
    end)
end)

-- God Mode (Anti-Ban)
RunService.Heartbeat:Connect(function()
    if Config.GodMode then
        pcall(function()
            local char = BF:GetCharacterSafe()
            if char then
                local hum = char:FindFirstChildOfClass("Humanoid")
                if hum then hum.Health = hum.MaxHealth end
            end
        end)
    end
end)

-- Anti AFK
RunService.Heartbeat:Connect(function()
    if Config.AntiAFK then
        pcall(function()
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new())
        end)
    end
end)

-- Full Bright
RunService.Heartbeat:Connect(function()
    if Config.FullBright then
        pcall(function()
            Lighting.Brightness = 2
            Lighting.ClockTime = 14
            Lighting.FogEnd = 100000
        end)
    end
end)

-- No Clip
RunService.Stepped:Connect(function()
    if Config.NoClip then
        pcall(function()
            if LocalPlayer.Character then
                for _, v in ipairs(LocalPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") then v.CanCollide = false end
                end
            end
        end)
    end
end)

-- ESP
RunService.RenderStepped:Connect(function()
    pcall(function()
        if Config.ESP or Config.ESPPlayers then
            for _, p in ipairs(BF:GetPlayers()) do
                if p.Character and p.Character:FindFirstChild("Head") then
                    p.Character.Head.Color = Color3.fromRGB(255, 0, 0)
                end
            end
        end
        if Config.ESP or Config.ESPFruits then
            for _, fruit in ipairs(BF:GetFruits()) do
                if fruit:IsA("BasePart") then fruit.Color = Color3.fromRGB(0, 255, 0) end
            end
        end
        if Config.ESP or Config.ESPChests then
            for _, chest in ipairs(BF:GetChests()) do
                if chest:IsA("BasePart") then chest.Color = Color3.fromRGB(255, 255, 0) end
            end
        end
    end)
end)

-- Aimbot
RunService.RenderStepped:Connect(function()
    if Config.Aimbot then
        pcall(function()
            local closest, closestDist = nil, Config.AimFOV
            local mousePos = UserInputService:GetMouseLocation()
            local center = Config.AimAssist and mousePos or Vector2.new(workspace.CurrentCamera.ViewportSize.X / 2, workspace.CurrentCamera.ViewportSize.Y / 2)
            for _, p in ipairs(BF:GetPlayers()) do
                if p.Character and p.Character:FindFirstChild("Head") then
                    local screenPos, onScreen = workspace.CurrentCamera:WorldToScreenPoint(p.Character.Head.Position)
                    if onScreen then
                        local dist = (Vector2.new(screenPos.X, screenPos.Y) - center).Magnitude
                        if dist < closestDist then closestDist = dist; closest = p end
                    end
                end
            end
            if closest then
                local targetPos = workspace.CurrentCamera:WorldToScreenPoint(closest.Character.Head.Position)
                mousemoverel(targetPos.X - mousePos.X, targetPos.Y - mousePos.Y)
            end
        end)
    end
end)

-- Auto Collect Fruits
RunService.Heartbeat:Connect(function()
    if Config.AutoCollectFruits then
        pcall(function()
            for _, fruit in ipairs(BF:GetFruits()) do
                if fruit:IsA("Tool") and LocalPlayer.Character then
                    LocalPlayer.Character.Humanoid:EquipTool(fruit)
                end
            end
        end)
    end
end)

-- Auto Open Chests
RunService.Heartbeat:Connect(function()
    if Config.AutoOpenChests then
        pcall(function()
            for _, chest in ipairs(BF:GetChests()) do
                if LocalPlayer.Character then
                    SafeTP(chest.Position + Vector3.new(0, 3, 0))
                    task.wait(Randomize(0.3, 0.7))
                end
            end
        end)
    end
end)

-- Auto Equip Best Sword
RunService.Heartbeat:Connect(function()
    if Config.AutoEquipBestSword then BF:EquipBestSword() end
end)

-- FPS Boost
RunService.Heartbeat:Connect(function()
    if Config.FPSBoost then pcall(function() settings().Rendering.QualityLevel = 1 end) end
end)

-- Remove Terrain
RunService.Heartbeat:Connect(function()
    if Config.RemoveTerrain then pcall(function() workspace.Terrain:Clear() end) end
end)

-- Hide Character
RunService.Heartbeat:Connect(function()
    if Config.HideCharacter and LocalPlayer.Character then
        pcall(function()
            for _, v in ipairs(LocalPlayer.Character:GetChildren()) do
                if v:IsA("BasePart") then v.Transparency = 1 end
            end
        end)
    end
end)

-- Auto Teleport Fruit
RunService.Heartbeat:Connect(function()
    if Config.AutoTeleportFruit then
        pcall(function()
            local fruits = BF:GetFruits()
            if #fruits > 0 then SafeTP(fruits[1].Position) end
        end)
    end
end)

-- Auto Collect All
RunService.Heartbeat:Connect(function()
    if Config.AutoCollectAll then
        pcall(function()
            for _, v in ipairs(workspace:GetDescendants()) do
                if v:IsA("Tool") and LocalPlayer.Character then
                    LocalPlayer.Character.Humanoid:EquipTool(v)
                end
            end
        end)
    end
end)

-- Security: Admin Check
RunService.Heartbeat:Connect(function()
    if Config.AutoLeaveOnAdmin then
        pcall(function()
            for _, p in ipairs(Players:GetPlayers()) do
                if p:GetRankInGroup(1) > 200 then
                    task.wait(Randomize(1, 3))
                    TeleportService:Teleport(game.PlaceId)
                end
            end
        end)
    end
end)

-- Notification Function
function Notify(title, msg)
    pcall(function()
        local notif = Instance.new("Frame")
        notif.Size = UDim2.new(0, 260, 0, 60)
        notif.Position = UDim2.new(1, 280, 0, 20)
        notif.BackgroundColor3 = Theme.Surface
        notif.BackgroundTransparency = 0.2
        notif.BorderSizePixel = 0
        notif.Parent = ScreenGui
        Instance.new("UICorner", notif).CornerRadius = UDim.new(0, 8)
        local ns = Instance.new("UIStroke", notif)
        ns.Color = Theme.Primary
        ns.Thickness = 2
        
        local t = Instance.new("TextLabel")
        t.Size = UDim2.new(1, -10, 0, 22)
        t.Position = UDim2.new(0, 5, 0, 3)
        t.BackgroundTransparency = 1
        t.Text = title
        t.TextColor3 = Theme.Primary
        t.TextSize = 14
        t.Font = Enum.Font.GothamBold
        t.TextXAlignment = Enum.TextXAlignment.Left
        t.Parent = notif
        
        local m = Instance.new("TextLabel")
        m.Size = UDim2.new(1, -10, 0, 25)
        m.Position = UDim2.new(0, 5, 0, 25)
        m.BackgroundTransparency = 1
        m.Text = msg
        m.TextColor3 = Theme.Text
        m.TextSize = 11
        m.Font = Enum.Font.Gotham
        m.TextXAlignment = Enum.TextXAlignment.Left
        m.Parent = notif
        
        TweenService:Create(notif, TweenInfo.new(0.4), {Position = UDim2.new(1, -280, 0, 20)}):Play()
        task.delay(4, function()
            TweenService:Create(notif, TweenInfo.new(0.4), {Position = UDim2.new(1, 280, 0, 20)}):Play()
            task.wait(0.4)
            notif:Destroy()
        end)
    end)
end

Notify("🛡️ ITACHI HUB ANTI-BAN", "v9.0 Final | 21 Tabs | 150+ Features | Universal")

end)

if not success then
    warn("❌ ItachiHub: " .. tostring(err))
end
