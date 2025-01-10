local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/memejames/elerium-v2-ui-library/main/Library", true))()

local window = library:AddWindow("Auto Kill Test", {
	main_color = Color3.fromRGB(41, 74, 122), -- Color
	min_size = Vector2.new(450, 420), -- Size of the GUI
	can_resize = false, -- true or false
})

local Killing = window:AddTab("Killing")

local autoKillEnabled = false
local hitboxSize = 5  -- Set the size of the hitbox part to 5

-- Function to create and follow a player's character
local function createFollowPart(player)
    -- Create a part
    local followPart = Instance.new("Part")
    followPart.Size = Vector3.new(hitboxSize, hitboxSize, hitboxSize)  -- Set the part size to 5
    followPart.Transparency = 1  -- Make the part invisible
    followPart.Material = Enum.Material.SmoothPlastic  -- Set material to SmoothPlastic
    followPart.CanCollide = false
    followPart.Anchored = false
    followPart.Parent = workspace  -- Parent it to the workspace

    -- Create a Weld to attach it to the player's HumanoidRootPart
    local weld = Instance.new("WeldConstraint")
    weld.Parent = followPart
    weld.Part0 = followPart
    weld.Part1 = player.Character:WaitForChild("HumanoidRootPart")

    -- Update the part's position every frame to follow the player
    game:GetService('RunService').RenderStepped:Connect(function()
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            followPart.CFrame = player.Character.HumanoidRootPart.CFrame  -- Position it at the root part
        end
    end)

    return followPart
end

-- Add switch to enable/disable Auto Kill Aura
Killing:AddSwitch("Auto Kill", function(State)
    autoKillEnabled = State
end)

game:GetService('RunService').RenderStepped:Connect(function()
    if autoKillEnabled then
        -- Iterate through all players and create the follow part for each one
        for _, player in pairs(game:GetService("Players"):GetPlayers()) do
            if player.Name ~= game.Players.LocalPlayer.Name then
                if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    -- Create a follow part for each player
                    if not player.Character:FindFirstChild("FollowPart") then
                        local followPart = createFollowPart(player)
                        followPart.Name = "FollowPart"
                    end
                end
            end
        end
    end
end)

-- Optional function to reset the player's character
local function resetPlayer()
    local player = game.Players.LocalPlayer
    if player.Character then
        player.Character:BreakJoints()  -- Break joints to reset the character (if necessary)
    end
end

-- Call the function to reset the character (if required by the script's logic)
resetPlayer()
