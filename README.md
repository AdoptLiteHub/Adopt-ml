local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/memejames/elerium-v2-ui-library/main/Library", true))()

local window = library:AddWindow("Auto Kill Test", {
    main_color = Color3.fromRGB(41, 74, 122),
    min_size = Vector2.new(450, 420),
    can_resize = false,
})

local Killing = window:AddTab("Killing")

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

-- Main loop that runs every frame to handle kill logic and punching
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
end)

-- Function to reset the player's character (optional)
local function resetPlayer()
    local player = game.Players.LocalPlayer
    if player.Character then
        player.Character:BreakJoints()
    end
end

resetPlayer()
