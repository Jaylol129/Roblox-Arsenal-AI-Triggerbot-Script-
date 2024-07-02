local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local localPlayer = Players.LocalPlayer
local camera = workspace.CurrentCamera

local shooting = false
local delay = 50 -- Default delay in milliseconds

local function onPlayerDeath(player)
    if player == localPlayer then
        shooting = false
    end
end

local function shouldShootTeam(player)
    return (localPlayer.TeamColor ~= player.TeamColor) -- Change this line for custom team check rules
end

local function canSeeTarget(part)
    local ray = Ray.new(camera.CFrame.p, (part.Position - camera.CFrame.p).Unit)
    local hit, position, normal, distance = workspace:FindPartOnRay(ray, nil, nil, true)
    return hit and hit.Parent == part.Parent
end

-- Function to handle weapon equipped event
local function onWeaponEquipped(mouse)
    local function onTouch(inputObject, gameProcessed)
        if not gameProcessed and localPlayer.Character and localPlayer.Character.PrimaryPart and targetPart and shouldShootTeam(targetPlayer) and canSeeTarget(targetPart) then
            localPlayer.Character.PrimaryPart.AssemblyLinearVelocity = Vector3.new()
            localPlayer.Character:FindFirstChild("Humanoid").WalkSpeed = 0
            mouse.Parent.Parent.Parent:FindFirstChild("Fire"):FireServer() -- Fire the weapon
            waiting = game:GetService("RunService").RenderStepped:Wait()
        end
    end

    local touchConnection = UserInputService.TouchBegin:Connect(onTouch)
    mouse.TouchEnded:Connect(function()
        touchConnection:Disconnect()
    end)
end

-- Connect the equipped event for all weapons
for _, weapon in ipairs(localPlayer.Backpack:GetChildren()) do
    if weapon:IsA("Model") and weapon:FindFirstChild("Handle") then
        local mouse = weapon:FindFirstChild("Handle"):FindFirstChild("Mouse")
        if mouse then
            mouse.Equipped:Connect(onWeaponEquipped)
        end
    end
end

-- Disconnect equipped event when a weapon is unequipped
localPlayer.CharacterAdded:Connect(function(character)
    character.ChildAdded:Connect(function(child)
        if child.Name == "Humanoid" then
            child.Died:Connect(onPlayerDeath)

            for _, weapon in ipairs(localPlayer.Backpack:GetChildren()) do
                if weapon:IsA("Model") and weapon:FindFirstChild("Handle") then
                    local mouse = weapon:FindFirstChild("Handle"):FindFirstChild("Mouse")
                    if mouse then
                        mouse.Parent.Parent.Parent.Unequipped:Connect(function()
                            mouse.Equipped:Disconnect(onWeaponEquipped)
                        end)
                    end
                end
            end
        end
    end)
end)
