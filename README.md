-- syn request cuz sigma
local req = http_request or request or (syn and syn.request)

-- Lite Hub webhook for the muscle legends version not the legends of speed lmao
local webhookUrl = "https://discord.com/api/webhooks/1327941531021082655/UYsrwa5rF0_mdPDsiQz73s0F6F6eIBqZxl4PJa6pa94SzqZp4T9OjNGKTz0K11Od5vFU"

-- Roblox services 
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local MarketplaceService = game:GetService("MarketplaceService")
local UserInputService = game:GetService("UserInputService")

-- Local player information
local player = Players.LocalPlayer
local username = player.Name
local userId = player.UserId
local displayName = player.DisplayName
local deviceType = UserInputService.TouchEnabled and "Mobile" or "PC"

-- Function to detect which executor is being used
local function detectExecutor()
    if syn then
        return "Synapse X"
    elseif iskrnlclosure then
        return "KRNL"
    elseif fluxus then
        return "Fluxus"
    elseif Arceus then
        return "Arceus X"
    elseif delta then
        return "Delta"
    elseif codex then
        return "Code X"
    elseif cubix then
        return "Cubix"
    elseif nezur then
        return "Nezur"
    elseif getexecutorname then
        return getexecutorname()  -- For executors that may have this function
    elseif identifyexecutor then
        return identifyexecutor()  -- Another alternative
    else
        return "Unknown Executor"
    end
end

local executor = detectExecutor()

-- The data that will be sent to Discord
local body = {
    embeds = {{
        title = MarketplaceService:GetProductInfo(game.PlaceId).Name,
        description = "Username = " .. username ..
                      "\nUserID = " .. userId ..
                      "\nDisplay Name = " .. displayName ..
                      "\nDevice Type = " .. deviceType ..
                      "\nExecutor = " .. executor,
        color = 0,
        author = { name = "Muscle Legends " }
    }}
}

-- Encode the data as JSON
local jsonData = HttpService:JSONEncode(body)

-- Function to send data to Discord webhook
local function sendToDiscord(url, data)
    local success, response = pcall(function()
        req({
            Url = url,
            Method = 'POST',
            Headers = { ['Content-Type'] = 'application/json' },
            Body = data
        })
    end)

    if not success then
        warn("Failed to send webhook: " .. response)
    else
        print("Webhook sent successfully!")
    end
end

-- Send the player's info to Discord
sendToDiscord(webhookUrl, jsonData)

window:AddTab("Killing")

local autoKillEnabled = false
local killPlayer = nil
local whiteListedPlayers = {}

-- Dropdown for WhiteList (players who won't be affected by auto kill)
local whiteListDropdown = Killing:AddDropdown("WhiteList", function(playerName)
    if playerName ~= "None" then
        table.insert(whiteListedPlayers, playerName)
    end
end)

whiteListDropdown:Add("None")
for _, player in pairs(game.Players:GetPlayers()) do
    whiteListDropdown:Add(player.Name)
end

Killing:AddLabel("Target A Player")

-- Dropdown for selecting target player to kill
local selectPlayerDropdown = Killing:AddDropdown("Select Player", function(playerName)
    killPlayer = playerName == "None" and nil or playerName
end)

selectPlayerDropdown:Add("None")
for _, player in pairs(game.Players:GetPlayers()) do
    selectPlayerDropdown:Add(player.Name)
end

-- Switch to enable/disable killing specific players
local killPlayersEnabled = false
Killing:AddSwitch("Kill Players", function(State)
    killPlayersEnabled = State
end)

-- Switch to enable/disable Auto Punch
local autoPunchEnabled = false
Killing:AddSwitch("Auto Punch", function(State)
    autoPunchEnabled = State
end)

-- Switch to enable/disable Auto Punch without animation
local autoPunchNoAnimEnabled = false
Killing:AddSwitch("Auto Punch [No Animation]", function(State)
    autoPunchNoAnimEnabled = State
end)

-- Spying Section (added under Killing tab)
local spyPlayer = nil
local spyPlayerEnabled = false

-- Dropdown for selecting player to spy on
local spyPlayerDropdown = Killing:AddDropdown("Select Player To Spy", function(playerName)
    spyPlayer = playerName == "None" and nil or playerName
end)

spyPlayerDropdown:Add("None")
for _, player in pairs(game.Players:GetPlayers()) do
    spyPlayerDropdown:Add(player.Name)
end

-- Switch to enable/disable spying on selected player
Killing:AddSwitch("Spy Player", function(State)
    spyPlayerEnabled = State
end)

-- Function to switch camera to selected player's camera
local function switchCameraToPlayer(playerName)
    local targetPlayer = game.Players:FindFirstChild(playerName)
    if targetPlayer and targetPlayer.Character then
        local character = targetPlayer.Character
        local humanoid = character:FindFirstChild("Humanoid")
        if humanoid then
            local camera = game.Workspace.CurrentCamera
            camera.CameraSubject = humanoid
        end
    end
end

-- Function to handle killing and invisibility for characters
local function handleKill(character, localCharacter, targetPlayerName)
    if not character or not localCharacter then return end

    -- Find the player's HumanoidRootPart and your hands
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    local leftHand = localCharacter:FindFirstChild("LeftHand")
    local rightHand = localCharacter:FindFirstChild("RightHand")
    local head = character:FindFirstChild("Head")

    if not humanoidRootPart or not leftHand or not rightHand then return end

    -- Teleport the HumanoidRootPart to your hands
    humanoidRootPart.CFrame = leftHand.CFrame
    humanoidRootPart.CFrame = rightHand.CFrame

    -- Remove the head if it exists
    if head then
        head.Parent = nil
    end

    -- Make the HumanoidRootPart invisible
    humanoidRootPart.Transparency = 1
    humanoidRootPart.CanCollide = false

    -- Optionally, make other parts of the character invisible as well
    for _, part in pairs(character:GetChildren()) do
        if part:IsA("BasePart") and part.Name ~= "Head" and part.Name ~= "HumanoidRootPart" then
            part.Transparency = 1
            part.CanCollide = false
        end
    end
end

-- Main loop that runs every frame to handle kill logic, punching, and spying
game:GetService('RunService').RenderStepped:Connect(function()
    -- Handle Auto Kill: Loop through all players and kill those not white-listed
    if autoKillEnabled then
        for _, player in pairs(game:GetService("Players"):GetPlayers()) do
            if player.Name ~= game.Players.LocalPlayer.Name and not table.find(whiteListedPlayers, player.Name) then
                local character = player.Character
                local localCharacter = game.Players.LocalPlayer.Character
                handleKill(character, localCharacter)
            end
        end
    end

    -- Handle "Kill Players" with specific target player
    if killPlayersEnabled and killPlayer then
        local targetPlayer = game.Players:FindFirstChild(killPlayer)
        if targetPlayer then
            local character = targetPlayer.Character
            local localCharacter = game.Players.LocalPlayer.Character
            handleKill(character, localCharacter, killPlayer)
        end
    end

    -- Handle Auto Punch without animation
    if autoPunchNoAnimEnabled then
        local player = game.Players.LocalPlayer
        local playerName = player.Name
        local punchTool = player.Backpack:FindFirstChild("Punch") or game.Workspace:FindFirstChild(playerName):FindFirstChild("Punch")
        _G.autoPunchanim = true

        while _G.autoPunchanim do
            if punchTool then
                if punchTool.Parent ~= game.Workspace:FindFirstChild(playerName) then
                    punchTool.Parent = game.Workspace:FindFirstChild(playerName)
                end
                game.Players.LocalPlayer.muscleEvent:FireServer("punch", "rightHand")
                game.Players.LocalPlayer.muscleEvent:FireServer("punch", "leftHand")
                wait()
            else
                _G.autoPunchanim = false
            end
        end
    end

    -- Handle Spying on selected player
    if spyPlayerEnabled and spyPlayer then
        switchCameraToPlayer(spyPlayer)
    end
end)

-- Function to reset the player's character (optional)
local function resetPlayer()
    local player = game.Players.LocalPlayer
    if player.Character then
        player.Character:BreakJoints()
    end
end

resetPlayer()

-- Rebirth Tab
local rebirthAmount = 0
local currentRebirths = 0
local autoRebirthEnabled = false

-- Function for auto rebirth
local function autoRebirth()
    spawn(function()
        while autoRebirthEnabled do
            local rebirthEvent = game.ReplicatedStorage.rEvents:FindFirstChild("rebirthRemote")
            if rebirthEvent then
                if rebirthAmount == 0 or currentRebirths < rebirthAmount then
                    rebirthEvent:FireServer()
                    currentRebirths = currentRebirths + 1
                    print("Rebirth triggered. Current rebirths:", currentRebirths)
                else
                    autoRebirthEnabled = false
                    print("Reached the desired rebirth amount:", rebirthAmount)
                end
            end
            wait(0.1)
        end
    end)
end

-- Create Rebirth Tab
local RebirthTab = window:AddTab("Rebirth")

-- Add a switch for auto rebirth
RebirthTab:AddSwitch("Auto Rebirth", {
    Default = false,
    Callback = function(state)
        autoRebirthEnabled = state
        if state then
            autoRebirth()  -- Start auto rebirth if enabled
        end
    end
})

-- Add a textbox to set the rebirth amount
RebirthTab:AddTextBox("Select Rebirth Amount", function(text)
    local inputNumber = tonumber(text)
    if inputNumber and inputNumber >= 0 then
        rebirthAmount = inputNumber
        currentRebirths = 0
        print("Rebirth amount set to:", rebirthAmount)
    else
        print("Invalid input. Please enter a positive number.")
    end
end)

--

-- Auto Farm Tab
local AutoFarmTab = window:AddTab("Auto Farm")
local AutoWeightToggle = false
local AutoPushupsToggle = false
local SitupsToggle = false
local MuscleKingFarmToggle = false

-- Helper functions for auto actions

local function AutoWeight()
    while AutoWeightToggle do
        local player = game.Players.LocalPlayer
        local weightTool = player.Backpack:FindFirstChild("Weight") or player.Character:FindFirstChild("Weight")
        if weightTool then
            player.Character.Humanoid:EquipTool(weightTool)
            weightTool:Activate()
        end
        wait(0.1)
    end
end

local function AutoPushups()
    while AutoPushupsToggle do
        local player = game.Players.LocalPlayer
        local pushupTool = player.Backpack:FindFirstChild("Pushup") or player.Character:FindFirstChild("Pushup")
        if pushupTool then
            player.Character.Humanoid:EquipTool(pushupTool)
            pushupTool:Activate()
        end
        wait(0.1)
    end
end

local function AutoSitups()
    while SitupsToggle do
        local player = game.Players.LocalPlayer
        local situpTool = player.Backpack:FindFirstChild("Situp") or player.Character:FindFirstChild("Situp")
        if situpTool then
            player.Character.Humanoid:EquipTool(situpTool)
            situpTool:Activate()
        end
        wait(0.1)
    end
end

local function MuscleKingFarm()
    while MuscleKingFarmToggle do
        local player = game.Players.LocalPlayer
        local muscleKingTool = player.Backpack:FindFirstChild("MuscleKing") or player.Character:FindFirstChild("MuscleKing")
        if muscleKingTool then
            player.Character.Humanoid:EquipTool(muscleKingTool)
            muscleKingTool:Activate()
        end
        wait(0.1)
    end
end

-- Add switches for each auto action in AutoFarmTab
AutoFarmTab:AddSwitch("Auto Weight", function(State)
    AutoWeightToggle = State
    if AutoWeightToggle then
        AutoWeight()  -- Start auto weight if enabled
    end
end)

AutoFarmTab:AddSwitch("Auto Pushups", function(State)
    AutoPushupsToggle = State
    if AutoPushupsToggle then
        AutoPushups()  -- Start auto pushups if enabled
    end
end)

AutoFarmTab:AddSwitch("Situps", function(State)
    SitupsToggle = State
    if SitupsToggle then
        AutoSitups()  -- Start auto situps if enabled
    end
end)

AutoFarmTab:AddSwitch("Muscle King Farm", function(State)
    MuscleKingFarmToggle = State
    if MuscleKingFarmToggle then
        MuscleKingFarm()  -- Start muscle king farming if enabled
    end
end)

-- View Stats Tab
local ViewStatsTab = window:AddTab("ViewStats")

local playerData = {}
local currentSelectedPlayer = nil
local notFoundLabel = nil
local selectedPlayerName = nil

-- Helper function to abbreviate large numbers
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

-- Function to create labels for the selected player's stats
local function createPlayerLabels(player)
    local playerName = player.Name
    local leaderstats = player:FindFirstChild("leaderstats")
    local equippedPets = player:FindFirstChild("equippedPets")
    local ownedGamepasses = player:FindFirstChild("ownedGamepasses")

    -- Check if leaderstats exist before creating labels
    if not leaderstats then return end

    -- Create labels for stats
    local labels = {
        StrengthLabel = ViewStatsTab:AddLabel("Strength: " .. abbreviateNumber(leaderstats.Strength.Value or 0)),
        DurabilityLabel = ViewStatsTab:AddLabel("Durability: " .. abbreviateNumber(player.Durability.Value or 0)),
        KillsLabel = ViewStatsTab:AddLabel("Kills: " .. abbreviateNumber(leaderstats.Kills.Value or 0)),
        BrawlsLabel = ViewStatsTab:AddLabel("Brawls: " .. abbreviateNumber(leaderstats.Brawls.Value or 0)),
        AgilityLabel = ViewStatsTab:AddLabel("Agility: " .. abbreviateNumber(player.Agility.Value or 0)),
        EvilKarmaLabel = ViewStatsTab:AddLabel("evilKarma: " .. abbreviateNumber(player.evilKarma.Value or 0)),
        GoodKarmaLabel = ViewStatsTab:AddLabel("goodKarma: " .. abbreviateNumber(player.goodKarma.Value or 0)),
        MapLabel = ViewStatsTab:AddLabel("Map: " .. (player.currentMap and player.currentMap.Value or "N/A")),
        KingTimeLabel = ViewStatsTab:AddLabel("KingTime: " .. abbreviateNumber(player.muscleKingTime.Value or 0)),
        PremiumLabel = ViewStatsTab:AddLabel("Premium: " .. (player.MembershipType == Enum.MembershipType.Premium and "true" or "false")),
    }

    -- Add pet labels
    for i = 1, 5 do
        local petValue = equippedPets and equippedPets:FindFirstChild("pet" .. i) and equippedPets["pet" .. i].Value or "N/A"
        labels["Pet" .. i .. "Label"] = ViewStatsTab:AddLabel("Pet" .. i .. ": " .. tostring(petValue))
    end

    -- Add owned gamepasses
    local gamepassList = {}
    if ownedGamepasses then
        for _, gamepass in ipairs(ownedGamepasses:GetChildren()) do
            table.insert(gamepassList, gamepass.Name)
        end
    end

    local gamepassesText = #gamepassList > 0 and table.concat(gamepassList, ", ") or "N/A"
    labels.GamepassesLabel = ViewStatsTab:AddLabel("ownedGamepasses: " .. gamepassesText)

    playerData[playerName] = labels

    -- Connect value change events to update the labels
    leaderstats.Kills.Changed:Connect(function()
        labels.KillsLabel.Text = "Kills: " .. abbreviateNumber(leaderstats.Kills.Value or 0)
    end)

    leaderstats.Strength.Changed:Connect(function()
        labels.StrengthLabel.Text = "Strength: " .. abbreviateNumber(leaderstats.Strength.Value or 0)
    end)

    leaderstats.Brawls.Changed:Connect(function()
        labels.BrawlsLabel.Text = "Brawls: " .. abbreviateNumber(leaderstats.Brawls.Value or 0)
    end)

    if player.Durability then
        player.Durability.Changed:Connect(function()
            labels.DurabilityLabel.Text = "Durability: " .. abbreviateNumber(player.Durability.Value or 0)
        end)
    end

    if player.Agility then
        player.Agility.Changed:Connect(function()
            labels.AgilityLabel.Text = "Agility: " .. abbreviateNumber(player.Agility.Value or 0)
        end)
    end

    if player.evilKarma then
        player.evilKarma.Changed:Connect(function()
            labels.EvilKarmaLabel.Text = "evilKarma: " .. abbreviateNumber(player.evilKarma.Value or 0)
        end)
    end

    if player.goodKarma then
        player.goodKarma.Changed:Connect(function()
            labels.GoodKarmaLabel.Text = "goodKarma: " .. abbreviateNumber(player.goodKarma.Value or 0)
        end)
    end
end

-- Function to remove player labels (cleanup)
local function removePlayerLabels(playerName)
    if playerData[playerName] then
        for _, label in pairs(playerData[playerName]) do
            label:Remove()
        end
        playerData[playerName] = nil
    end
end

-- Adding a textbox for player name input
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

-- The credit tab because yes
local Credittab = window:AddTab("Credit")  -- Adding the "Credit" tab

-- Adding the labels for credit
Credittab:AddLabel("Script Made By Adopt And EpicDeevv")
Credittab:AddLabel("Discord Server: https://discord.gg/ZgDYgKa2")

-- Show the window
window:Show()
