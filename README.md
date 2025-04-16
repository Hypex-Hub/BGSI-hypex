-- Create GUI Elements
local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
screenGui.ResetOnSpawn = false

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 250, 0, 300)
frame.Position = UDim2.new(0.5, -125, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Active = true
frame.Draggable = true

-- Toggle Button
local toggle = Instance.new("TextButton", frame)
toggle.Size = UDim2.new(1, 0, 0, 30)
toggle.Position = UDim2.new(0, 0, 0, 0)
toggle.Text = "Turn ON"
toggle.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggle.TextColor3 = Color3.new(1,1,1)

-- Container for action buttons
local buttonHolder = Instance.new("Frame", frame)
buttonHolder.Position = UDim2.new(0, 0, 0, 40)
buttonHolder.Size = UDim2.new(1, 0, 1, -40)
buttonHolder.BackgroundTransparency = 1

local enabled = false

-- Function to fire the RemoteEvent
local function fireShop(shopName, index)
    if not enabled then return end

    local args = {
        [1] = "BuyShopItem";
        [2] = shopName;
        [3] = index;
    }

    local remoteEvent = game:GetService("ReplicatedStorage"):WaitForChild("Shared", 9e9)
        :WaitForChild("Framework", 9e9):WaitForChild("Network", 9e9)
        :WaitForChild("Remote", 9e9):WaitForChild("Event", 9e9)

    remoteEvent:FireServer(unpack(args))
    task.wait(5)
end

-- Create buttons for all 6 options
local data = {
    {"alien-shop", 1},
    {"alien-shop", 2},
    {"alien-shop", 3},
    {"shard-shop", 1},
    {"shard-shop", 2},
    {"shard-shop", 3},
}

for i, entry in ipairs(data) do
    local btn = Instance.new("TextButton", buttonHolder)
    btn.Size = UDim2.new(1, -20, 0, 30)
    btn.Position = UDim2.new(0, 10, 0, (i - 1) * 35)
    btn.Text = entry[1] .. " - " .. entry[2]
    btn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    btn.TextColor3 = Color3.new(1,1,1)

    btn.MouseButton1Click:Connect(function()
        fireShop(entry[1], entry[2])
    end)
end

-- Toggle on/off
toggle.MouseButton1Click:Connect(function()
    enabled = not enabled
    toggle.Text = enabled and "Turn OFF" or "Turn ON"
    toggle.BackgroundColor3 = enabled and Color3.fromRGB(120, 50, 50) or Color3.fromRGB(50, 50, 50)
end)