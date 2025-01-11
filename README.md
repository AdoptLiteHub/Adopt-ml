local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/memejames/elerium-v2-ui-library/main/Library", true))()

local window = library:AddWindow("Auto Kill Test", {
	main_color = Color3.fromRGB(41, 74, 122), -- Color
	min_size = Vector2.new(450, 420), -- Size of the GUI
	can_resize = false, -- true or false
})

local Killing = window:AddTab("Killing")

local autoKillEnabled = false

-- Create the toggle switch for "Auto Kill"
Killing:AddSwitch("Auto Kill", function(State)
    autoKillEnabled = State
end)

-- Function to handle the "Auto Kill" process
game:GetService('RunService').RenderStepped:Connect(function()
    if autoKillEnabled then
        -- Iterate through all players in the game
        for _, player in pairs(game:GetService("Players"):GetPlayers()) do
            if player.Name ~= game.Players.LocalPlayer.Name then
                local character = player.Character
                local localCharacter = game.Players.LocalPlayer.Character

                if character and localCharacter then
                    local leftHand = localCharacter:FindFirstChild("LeftHand")
                    local rightHand = localCharacter:FindFirstChild("RightHand")

                    -- Ensure the hands exist and proceed
                    if leftHand and rightHand then
                        -- Make the other player's body invisible
                        for _, part in pairs(character:GetChildren()) do
                            if part:IsA("BasePart") and part.Name ~= "Head" then
                                part.Transparency = 1  -- Make the part invisible
                                part.CanCollide = false  -- Disable collision
                            end
                        end

                        -- Remove the head from the other player's character
                        local head = character:FindFirstChild("Head")
                        if head then
                            head.Parent = nil  -- Removes the head from the character
                        end

                        -- Teleport the other player's body parts to the local player's hands
                        local newPositionLeft = leftHand.Position
                        local newPositionRight = rightHand.Position

                        -- Move the body parts to the positions of the local player's hands
                        for _, part in pairs(character:GetChildren()) do
                            if part:IsA("BasePart") and part.Name ~= "Head" then
                                part.CFrame = CFrame.new(newPositionLeft)  -- Move to the left hand
                                part.CFrame = CFrame.new(newPositionRight)  -- Move to the right hand
                            end
                        end
                    end
                end
            end
        end

        -- Now handle the punching logic for the local player
        local player = game.Players.LocalPlayer
        local playerName = player.Name
        local punchTool =
            player.Backpack:FindFirstChild("Punch") or
            game.Workspace:FindFirstChild(playerName):FindFirstChild("Punch")
        _G.autoPunchanim = true -- Global control variable

        while _G.autoPunchanim do
            if punchTool then
                if punchTool.Parent ~= game.Workspace:FindFirstChild(playerName) then
                    punchTool.Parent = game.Workspace:FindFirstChild(playerName) -- Equip the tool
                end
                game.Players.LocalPlayer.muscleEvent:FireServer("punch", "rightHand")
                game.Players.LocalPlayer.muscleEvent:FireServer("punch", "leftHand")
                wait() -- Adjust the delay as needed for timing between punches
            else
                warn("Punch tool not found")
                _G.autoPunchanim = false -- Optional: Stop the loop if tool is not found
            end
        end
    end
end)

-- Optional function to reset the player's character (if required)
local function resetPlayer()
    local player = game.Players.LocalPlayer
    if player.Character then
        player.Character:BreakJoints()  -- Break joints to reset the character (if necessary)
    end
end

-- Call the function to reset the character (if required)
resetPlayer()
