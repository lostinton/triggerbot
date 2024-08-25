# triggerbot dh script dk if it works

go skid!!!

getgenv().prediction = 0.12
getgenv().click_delay = 0.001
getgenv().click_duration = 0.001

local players = game:GetService("Players")
local userInputService = game:GetService("UserInputService")
local localPlayer = players.LocalPlayer
local mouse = localPlayer:GetMouse()

local function click()
    mouse1press()
    wait(getgenv().click_duration)
    mouse1release()
end

local function predictPosition(target)
    if target and target.Parent and target.Parent:FindFirstChild("HumanoidRootPart") then
        local hrp = target.Parent.HumanoidRootPart
        local velocity = hrp.Velocity
        local futurePosition = hrp.Position + (velocity * getgenv().prediction)
        return futurePosition
    end
    return nil
end

local function triggerbot()
    while wait(getgenv().click_delay) do
        local target = mouse.Target
        if target and target.Parent and target.Parent:FindFirstChild("Humanoid") then
            local futurePosition = predictPosition(target)
            if futurePosition then
                mouse.Hit = CFrame.new(futurePosition)
                click()
            end
        end
    end
end

userInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.T then
        triggerbot()
    end
end)

print("Triggerbot loaded. Press 'T' to activate.")
