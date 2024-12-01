local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/AdoptLiteHub/Adopt-lib/refs/heads/main/README.md", true))()

local window = library:AddWindow("Muscle Legend Adopt", {
    main_color = Color3.fromRGB(0, 0, 0), -- Color
    min_size = Vector2.new(400, 346), -- Size of the gui
    can_resize = false, -- true or false
})

-- Variables
local defaultWalkSpeed = 16 
local defaultJumpPower = 50 
local defaultHipHeight = 2
local AutoPunchToggle = false
local playerNames = {}

-- Tabs
local Maintab = window:AddTab("Main") 

local KillTab = window:AddTab("Kill")

KillTab:AddButton("Speed Punch", function() 
    local player = game.Players.LocalPlayer 
    local punch = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch") 
    if punch and punch:FindFirstChild("attackTime") then 
        punch.attackTime.Value = 0 
    end 
end)

KillTab:AddSwitch("Auto Punch", function(State) 
    AutoPunchToggle = State 
    if AutoPunchToggle then 
        local function autoPunchLoop() 
            while AutoPunchToggle do 
                local player = game.Players.LocalPlayer 
                local tool = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch") 
                if tool and tool.Parent ~= player.Character then 
                    tool.Parent = player.Character 
                end 
                if tool then 
                    tool:Activate() 
                end 
                wait(0.01) 
            end 
        end 
        coroutine.wrap(autoPunchLoop)() 
    end 
end)

-- MainTab (Controls WalkSpeed, JumpPower, HipHeight)
Maintab:AddSlider("WalkSpeed", function(Value) 
    local player = game.Players.LocalPlayer 
    if player and player.Character then 
        player.Character.Humanoid.WalkSpeed = Value 
    end 
end, { Min = 16, Max = 2000, Default = defaultWalkSpeed }) 

Maintab:AddSlider("JumpPower", function(Value) 
    local player = game.Players.LocalPlayer 
    if player and player.Character then 
        player.Character.Humanoid.JumpPower = Value 
    end 
end, { Min = 50, Max = 500, Default = defaultJumpPower }) 

Maintab:AddSlider("HipHeight", function(Value) 
    local player = game.Players.LocalPlayer 
    if player and player.Character then 
        player.Character.Humanoid.HipHeight = Value 
    end 
end, { Min = 0, Max = 1000, Default = defaultHipHeight })

-- Rebirth Tab 
local rebirthAmount = 0 -- Store the desired rebirth amount 
local currentRebirths = 0 -- Track the number of rebirths performed 
local autoRebirthEnabled = false -- State of the Auto Rebirth toggle 

local function autoRebirth() 
    spawn(function() 
        while autoRebirthEnabled do 
            -- Check if the rebirthRemote exists 
            local rebirthevent = game.ReplicatedStorage.rEvents:FindFirstChild("rebirthRemote") 
            if rebirthevent then 
                -- Fire the rebirthRemote if the target hasn't been reached 
                if rebirthAmount == 0 or currentRebirths < rebirthAmount then 
                    rebirthevent:FireServer() 
                    currentRebirths = currentRebirths + 1 
                else 
                    -- Stop rebirthing if the target is reached 
                    autoRebirthEnabled = false 
                    print("Reached the desired rebirth amount.") 
                end 
            end 
            wait(0.1) -- Adjust delay as needed 
        end 
    end) 
end

-- Add a Rebirth Tab
local RebirthTab = window:AddTab("Rebirth") 

-- Add the Auto Rebirth Toggle
RebirthTab:AddSwitch("Auto Rebirth", { 
    Default = false, 
    Callback = function(state) 
        autoRebirthEnabled = state 
        if state then 
            autoRebirth() -- Start rebirthing when enabled 
        end 
    end 
}) 

-- Add a Textbox for Rebirth Amount 
RebirthTab:AddTextBox("Select Rebirth Amount", function(text) 
    local inputNumber = tonumber(text) -- Convert input text to a number 
    if inputNumber and inputNumber >= 0 then 
        rebirthAmount = inputNumber -- Set the target rebirth amount 
        currentRebirths = 0 -- Reset the counter 
        print("Rebirth amount set to:", rebirthAmount) 
    else 
        print("Invalid input. Please enter a positive number.") 
    end 
end) 

-- Misc Tab
local MiscTab = window:AddTab("Misc")

MiscTab:AddButton("Anti Crash", function() 
    wait(0.5)
    local ba = Instance.new("ScreenGui")
    local ca = Instance.new("TextLabel")
    local da = Instance.new("Frame")
    local _b = Instance.new("TextLabel")
    local ab = Instance.new("TextLabel")
    ba.Parent = game.CoreGui 
    ba.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    ca.Parent = ba 
    ca.Active = true
    ca.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
    ca.Draggable = true
    ca.Position = UDim2.new(0.698610067, 0, 0.098096624, 0)
    ca.Size = UDim2.new(0, 370, 0, 52) 
    ca.Font = Enum.Font.SourceSansSemibold 
    ca.Text = "Anti Afk"
    ca.TextColor3 = Color3.new(0, 1, 1)
    ca.TextSize = 22
    da.Parent = ca 
    da.BackgroundColor3 = Color3.new(0.196078, 0.196078, 0.196078)
    da.Position = UDim2.new(0, 0, 1.0192306, 0) 
    da.Size = UDim2.new(0, 370, 0, 107)
    _b.Parent = da
    _b.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
    _b.Position = UDim2.new(0, 0, 0.800455689, 0)
    _b.Size = UDim2.new(0, 370, 0, 21)
    _b.Font = Enum.Font.Arial
    _b.Text = "Made by luca#5432" 
    _b.TextColor3 = Color3.new(0, 1, 1)
    _b.TextSize = 20
    ab.Parent = da 
    ab.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
    ab.Position = UDim2.new(0, 0, 0.158377, 0) 
    ab.Size = UDim2.new(0, 370, 0, 44) 
    ab.Font = Enum.Font.ArialBold 
    ab.Text = "Status: Active"
    ab.TextColor3 = Color3.new(0, 1, 1)
    ab.TextSize = 20
    local bb = game:service'VirtualUser' 
    game:service'Players'.LocalPlayer.Idled:connect(function() 
        bb:CaptureController()
        bb:ClickButton2(Vector2.new())
        ab.Text = "Roblox tried kicking you but I didn't let them!"
        wait(2)
        ab.Text = "Status: Active"
    end) 
end)

MiscTab:AddButton("Destroy Ad teleport", function() 
    -- Find and destroy the part named "RobloxForwardPortals" 
    local part = workspace:FindFirstChild("RobloxForwardPortals") 
    if part then 
        part:Destroy() 
        print("Part 'RobloxForwardPortals' has been destroyed.") 
    else 
        print("Part 'RobloxForwardPortals' not found.") 
    end 
end)

-- AutoFarm Tab
-- AutoFarm Tab
local AutoFarmTab = window:AddTab("Auto Farm")

local AutoWeightToggle = false
local AutoPushupsToggle = false
local SitupsToggle = false
local MuscleKingFarmToggle = false

-- Auto Punch
local function AutoPunch() 
    spawn(function() 
        while AutoPunchToggle do 
            local player = game.Players.LocalPlayer 
            local punchTool = player.Backpack:FindFirstChild("Punch") or player.Character:FindFirstChild("Punch") 
            if punchTool then 
                player.Character.Humanoid:EquipTool(punchTool) 
                punchTool:Activate() 
            end 
            wait(0.1) 
        end 
    end) 
end

-- Auto Weight
local function AutoWeight() 
    spawn(function() 
        while AutoWeightToggle do 
            local player = game.Players.LocalPlayer 
            local weight = game.Workspace:FindFirstChild("Weight") -- Replace with the actual weight object name
            if weight then 
                -- Interact with the weight object
                weight:Activate() -- Example of activating the weight
            end 
            wait(0.5) 
        end 
    end) 
end

-- Auto Pushups (example function, need actual implementation)
local function AutoPushups()
    spawn(function()
        while AutoPushupsToggle do
            local player = game.Players.LocalPlayer
            local pushup = game.Workspace:FindFirstChild("Pushup") -- Replace with the correct pushup object
            if pushup then
                -- Simulate performing pushups
                pushup:Activate()
            end
            wait(0.5) -- Adjust the wait time to match desired frequency
        end
    end)
end

-- Muscle King Auto Farm (example function, need actual object references)
local function MuscleKingFarm()
    spawn(function()
        while MuscleKingFarmToggle do
            local player = game.Players.LocalPlayer
            local muscleKing = game.Workspace:FindFirstChild("MuscleKing") -- Replace with actual MuscleKing reference
            if muscleKing then
                -- Perform actions with the MuscleKing (e.g., activate or use it)
                muscleKing:Activate()
            end
            wait(0.5) -- Adjust the wait time for this farm action
        end
    end)
end

AutoFarmTab:AddSwitch("Auto Punch", function(State) 
    AutoPunchToggle = State 
    if AutoPunchToggle then 
        AutoPunch() 
    end 
end)

AutoFarmTab:AddSwitch("Auto Weight", function(State) 
    AutoWeightToggle = State 
    if AutoWeightToggle then 
        AutoWeight() 
    end 
end)

AutoFarmTab:AddSwitch("Situps", function(State)
    SitupsToggle = State
    -- Add functionality for SitupsToggle if needed
end)

AutoFarmTab:AddSwitch("MuscleKing Farm", function(State)
    MuscleKingFarmToggle = State
    if MuscleKingFarmToggle then
        MuscleKingFarm()
    end
end)

-- Misc Tab (Anti-Afk, Destroy Ad teleport)
local MiscTab = window:AddTab("Misc")

MiscTab:AddButton("Anti Crash", function() 
    wait(0.5)
    local ba = Instance.new("ScreenGui")
    local ca = Instance.new("TextLabel")
    local da = Instance.new("Frame")
    local _b = Instance.new("TextLabel")
    local ab = Instance.new("TextLabel")
    ba.Parent = game.CoreGui 
    ba.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    ca.Parent = ba 
    ca.Active = true
    ca.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
    ca.Draggable = true
    ca.Position = UDim2.new(0.698610067, 0, 0.098096624, 0)
    ca.Size = UDim2.new(0, 370, 0, 52) 
    ca.Font = Enum.Font.SourceSansSemibold 
    ca.Text = "Anti Afk"
    ca.TextColor3 = Color3.new(0, 1, 1)
    ca.TextSize = 22
    da.Parent = ca 
    da.BackgroundColor3 = Color3.new(0.196078, 0.196078, 0.196078)
    da.Position = UDim2.new(0, 0, 1.0192306, 0) 
    da.Size = UDim2.new(0, 370, 0, 107)
    _b.Parent = da
    _b.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
    _b.Position = UDim2.new(0, 0, 0.800455689, 0)
    _b.Size = UDim2.new(0, 370, 0, 21)
    _b.Font = Enum.Font.Arial
    _b.Text = "Made by luca#5432" 
    _b.TextColor3 = Color3.new(0, 1, 1)
    _b.TextSize = 20
    ab.Parent = da 
    ab.BackgroundColor3 = Color3.new(0.176471, 0.176471, 0.176471)
    ab.Position = UDim2.new(0, 0, 0.158377, 0) 
    ab.Size = UDim2.new(0, 370, 0, 44) 
    ab.Font = Enum.Font.ArialBold 
    ab.Text = "Status: Active"
    ab.TextColor3 = Color3.new(0, 1, 1)
    ab.TextSize = 20
    local bb = game:service'VirtualUser' 
    game:service'Players'.LocalPlayer.Idled:connect(function() 
        bb:CaptureController()
        bb:ClickButton2(Vector2.new())
        ab.Text = "Roblox tried kicking you but I didn't let them!"
        wait(2)
        ab.Text = "Status: Active"
    end) 
end)

MiscTab:AddButton("Destroy Ad teleport", function() 
    -- Find and destroy the part named "RobloxForwardPortals" 
    local part = workspace:FindFirstChild("RobloxForwardPortals") 
    if part then 
        part:Destroy() 
        print("Part 'RobloxForwardPortals' has been destroyed.") 
    else 
        print("Part 'RobloxForwardPortals' not found.") 
    end 
end)

-- Rock Tab
local RockTab = Window:AddTab("Rock")

-- Tiny Rock Teleport
RockTab:AddButton("Tiny Rock", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(17.6410236, -1.30998898, 2106.48926)
end)

-- Frozen Rock Teleport
RockTab:AddButton("Frozen Rock", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2551.75854, -0.359962642, -243.308777)
end)

-- Mystic Rock Teleport
RockTab:AddButton("Mystic Rock", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2186.14111, -0.359961987, 1250.59802)
end)

-- Inferno Rock Teleport
RockTab:AddButton("Inferno Rock", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-7262.18701, -0.359961987, -1259.24426)
end)

-- Legend Rock Teleport
RockTab:AddButton("Legend Rock", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(4140.41797, 987.453186, -4089.34937)
end)

-- MuscleKing Rock Teleport
RockTab:AddButton("Muscle King Rock", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-8971.56641, 27.4031715, -6061.27734)
end)

-- Teleport Tab
local TeleportTab = Window:AddTab("Teleport")

-- Beach Teleport
TeleportTab:AddButton("Beach", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-11, 5, -178)
end)

-- Legends Teleport
TeleportTab:AddButton("Legends", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(4603, 989, -3898)
end)

-- Muscle Teleport
TeleportTab:AddButton("Muscle", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-8626, 15, -5730)
end)

-- Tiny Teleport
TeleportTab:AddButton("Tiny", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-38, 5, 1884)
end)

-- Secret Teleport
TeleportTab:AddButton("Secret", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2596, -1, 5738)
end)

-- Inferno Teleport
TeleportTab:AddButton("Inferno", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-6759, 5, -1285)
end)

-- Frost Teleport
TeleportTab:AddButton("Frost", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2623, 5, -409)
end)

-- Mythical Teleport
TeleportTab:AddButton("Mythical", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2251, 5, 1073)
end)

-- View Stats Tab
local ViewStatsTab = Window:AddTab("ViewStats")

local playerData = {}
local currentSelectedPlayer = nil
local notFoundLabel = nil
local selectedPlayerName = nil

-- Function to abbreviate large numbers
local function abbreviateNumber(value)
    if value >= 1e15 then
        return string.format("%.1fQa", value / 1e15)
    elseif value >= 1e12 then
        return string.format("%.1fT", value / 1e12)
    elseif value >= 1e9 then
        return string.format("%.1fB", value / 1e9)
    elseif value >= 1e6 then
        return string.format("%.1fM", value / 1e6)
    elseif value >= 1e3 then
        return string.format("%.1fK", value / 1e3)
    else
        return tostring(value)
    end
end

-- Function to create player data labels
local function createPlayerLabels(player)
    local playerName = player.Name
    local leaderstats = player:FindFirstChild("leaderstats")
    local equippedPets = player:FindFirstChild("equippedPets")
    local ownedGamepasses = player:FindFirstChild("ownedGamepasses")

    local labels = {
        StrengthLabel = ViewStatsTab:AddLabel("Strength: " .. abbreviateNumber(leaderstats.Strength.Value or 0)),
        DurabilityLabel = ViewStatsTab:AddLabel("Durability: " .. abbreviateNumber(player.Durability.Value or 0)),
        KillsLabel = ViewStatsTab:AddLabel("Kills: " .. abbreviateNumber(leaderstats.Kills.Value or 0)),
        BrawlsLabel = ViewStatsTab:AddLabel("Brawls: " .. abbreviateNumber(leaderstats.Brawls.Value or 0)),
        AgilityLabel = ViewStatsTab:AddLabel("Agility: " .. abbreviateNumber(player.Agility.Value or 0)),
        EvilKarmaLabel = ViewStatsTab:AddLabel("evilKarma: " .. abbreviateNumber(player.evilKarma.Value or 0)),
        GoodKarmaLabel = ViewStatsTab:AddLabel("goodKarma: " .. abbreviateNumber(player.goodKarma.Value or 0)),
        MapLabel = ViewStatsTab:AddLabel("Map: " .. (player.currentMap.Value or "N/A")),
        KingTimeLabel = ViewStatsTab:AddLabel("KingTime: " .. abbreviateNumber(player.muscleKingTime.Value or 0)),
        PremiumLabel = ViewStatsTab:AddLabel("Premium: " .. (player.MembershipType == Enum.MembershipType.Premium and "true" or "false")),
    }

    for i = 1, 5 do
        local petValue = equippedPets:FindFirstChild("pet" .. i) and equippedPets["pet" .. i].Value or "N/A"
        labels["Pet" .. i .. "Label"] = ViewStatsTab:AddLabel("Pet" .. i .. ": " .. tostring(petValue))
    end

    local gamepassList = {}
    if ownedGamepasses then
        for _, gamepass in ipairs(ownedGamepasses:GetChildren()) do
            table.insert(gamepassList, gamepass.Name)
        end
    end

    local gamepassesText = #gamepassList > 0 and table.concat(gamepassList, ", ") or "N/A"
    labels.GamepassesLabel = ViewStatsTab:AddLabel("ownedGamepasses: " .. gamepassesText)

    playerData[playerName] = labels

    -- Update labels dynamically on stat changes
    leaderstats.Kills.Changed:Connect(function()
        labels.KillsLabel.Text = "Kills: " .. abbreviateNumber(leaderstats.Kills.Value or 0)
    end)

    leaderstats.Strength.Changed:Connect(function()
        labels.StrengthLabel.Text = "Strength: " .. abbreviateNumber(leaderstats.Strength.Value or 0)
    end)

    leaderstats.Brawls.Changed:Connect(function()
        labels.BrawlsLabel.Text = "Brawls: " .. abbreviateNumber(leaderstats.Brawls.Value or 0)
    end)

    player.Durability.Changed:Connect(function()
        labels.DurabilityLabel.Text = "Durability: " .. abbreviateNumber(player.Durability.Value or 0)
    end)

    player.Agility.Changed:Connect(function()
        labels.AgilityLabel.Text = "Agility: " .. abbreviateNumber(player.Agility.Value or 0)
    end)

    player.evilKarma.Changed:Connect(function()
        labels.EvilKarmaLabel.Text = "evilKarma: " .. abbreviateNumber(player.evilKarma.Value or 0)
    end)

    player.goodKarma.Changed:Connect(function()
        labels.GoodKarmaLabel.Text = "goodKarma: " .. abbreviateNumber(player.goodKarma.Value or 0)
    end)

    player.currentMap.Changed:Connect(function()
        labels.MapLabel.Text = "Map: " .. (player.currentMap.Value or "N/A")
    end)

    player.muscleKingTime.Changed:Connect(function()
        labels.KingTimeLabel.Text = "KingTime: " .. abbreviateNumber(player.muscleKingTime.Value or 0)
    end)
end

-- Function to remove player data labels
local function removePlayerLabels(playerName)
    local labels = playerData[playerName]
    if labels then
        for _, label in pairs(labels) do
            label:Remove()
        end
        playerData[playerName] = nil
    end
end

-- Textbox for entering player name to view stats
local textbox = ViewStatsTab:AddTextBox("Player Name", function(playerName)
    selectedPlayerName = playerName
    if notFoundLabel then
        notFoundLabel:Remove()
        notFoundLabel = nil
    end

    local player = game.Players:FindFirstChild(playerName)
    if player then
        if currentSelectedPlayer then
            removePlayerLabels(currentSelectedPlayer)
        end
        createPlayerLabels(player)
        currentSelectedPlayer = playerName
    else
        notFoundLabel = ViewStatsTab:AddLabel("Player not found!")
    end
end)

-- Spy Tab
local SpyTab = Window:AddTab("Spy")

local selectedPlayerName = nil -- To store the player name entered
local viewingPlayer = false -- To track whether the camera is locked on a player

-- Add TextBox for selecting a player
SpyTab:AddTextBox("Player Name", function(playerName)
    selectedPlayerName = playerName
end)

-- Add toggle to lock/unlock camera view
SpyTab:AddSwitch("Spy Player", function(bool)
    local camera = game.Workspace.CurrentCamera

    if bool then
        -- Enable camera lock
        if selectedPlayerName then
            local targetPlayer = game.Players:FindFirstChild(selectedPlayerName)
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                local humanoidRootPart = targetPlayer.Character.HumanoidRootPart
                viewingPlayer = true -- Enable camera following

                -- Loop to lock camera while the toggle is active
                while viewingPlayer and bool do
                    camera.CameraSubject = humanoidRootPart
                    wait()
                end
            end
        end
    else
        -- Disable camera lock and reset to the player's own character
        viewingPlayer = false
        camera.CameraSubject = game.Players.LocalPlayer.Character.Humanoid
    end
end)

local selectedPlayerForKill = nil -- To store the player name for instant kill

-- Add TextBox for selecting a player for Instant Kill
SpyTab:AddTextBox("Select Player", function(playerName)
    selectedPlayerForKill = playerName
end)

-- Add toggle to teleport the selected player above the void
SpyTab:AddSwitch("Instant Kill", function(bool)
    if bool then
        if selectedPlayerForKill then
            local targetPlayer = game.Players:FindFirstChild(selectedPlayerForKill)
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                -- Teleport the player 1 stud above the void
                targetPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(0, -4999, 0) -- Just above the void
            end
        end
    end
end)

-- Credits Tab
local CreditsTab = Window:AddTab("Credits")
CreditsTab:AddLabel("Script by Adopt, Cyber and EpicDeevv")
CreditsTab:AddLabel("Discord: curve0610/kr.1220/cyberivyontop")
