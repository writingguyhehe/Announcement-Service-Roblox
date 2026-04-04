local announcementService = {
	font = Enum.Font.FredokaOne,
	uiStroke = true,
	uiStrokeThickness = 3,
	adding = UDim2.new(0,0,0.05,0),
	easingStyle = Enum.EasingStyle.Quad,
}

local TweenService = game:GetService("TweenService")
local cautionSound = script.CautionSound
local popupSound = script.PopupSound
local ICON_RIGHT_NUDGE = -0.005 -- HOW FAR TO THE RIGHT THE CAUTION IMAGE WILL MOVE

announcementService.Announce = function 
	(player:Player, 
		announcementString:string, 
		announcementDuration:number, 
		tweenInfoDuration:number,
		textColor:Color3 -- OPTIONAL
	)
	
	local announcementText = script.AnnouncementText
	local announcementClone = announcementText:Clone()
	
	announcementClone.Visible = false
	
	local announcementUI = player.PlayerGui:WaitForChild("AnnouncementUI")
	local textTweenInfo = TweenInfo.new(
		tweenInfoDuration,
		announcementService.easingStyle
	)
	
	if announcementUI then
		announcementClone.Parent = announcementUI.MainFrame
		announcementClone.Text = announcementString
		announcementClone.UIStroke.Thickness = announcementService.uiStrokeThickness
		announcementClone.TextTransparency = 1
		announcementClone.UIStroke.Transparency = 1
		announcementClone.Visible = true
		
		if (not textColor) then
			textColor = Color3.fromRGB(255,255,255)
		end
		
		announcementClone.TextColor3 = textColor
		
		local showAnnouncement = TweenService:Create(
			announcementClone, 
			textTweenInfo, 
			{
				TextTransparency = 0
			}
		)
		
		if announcementService.uiStroke then
			local showUiStroke = TweenService:Create(
				announcementClone.UIStroke, 
				textTweenInfo, 
				{
					Transparency = 0
				}
			)
			showUiStroke:Play()
		end
		showAnnouncement:Play()
		popupSound:Play()
		
		task.wait(announcementDuration)
		
		local hideAnnouncement = TweenService:Create(
			announcementClone,
			textTweenInfo,
			{
				TextTransparency = 1
			}
		)
		
		if announcementService.uiStroke == true then
			local hideUiStroke = TweenService:Create(
				announcementClone.UIStroke,
				textTweenInfo,
				{
					Transparency = 1
				}
			)
			hideUiStroke:Play()
			hideAnnouncement:Play()
		end
		
		hideAnnouncement.Completed:Wait()
		announcementClone:Destroy()
	end
end


announcementService.AnnounceCaution = function 
	(
		player:Player, 
		announcementString:string, 
		announcementDuration:number, 
		tweenInfoDuration:number,
		textColor:Color3 -- OPTIONAL
	)
	
	local function positionIcon(cautionImg: ImageLabel, cautionLabel: TextLabel)
		task.wait()
		local textBounds = cautionLabel.TextBounds
		local labelWidth = cautionLabel.AbsoluteSize.X
		
		if labelWidth == 0 then
			return
		end
		
		local textStartXScale = (1 - (textBounds.X / labelWidth)) / 2
		cautionImg.AnchorPoint = Vector2.new(1, 0.5)
		cautionImg.Position = UDim2.new(textStartXScale + ICON_RIGHT_NUDGE, 0, 0.498, 0)
	end
	
	local cautionFrame = script.CautionFrame
	local cautionClone = cautionFrame:Clone()
	local cautionText = cautionClone.CautionText

	cautionText.Visible = false

	local announcementUI = player.PlayerGui:WaitForChild("AnnouncementUI")
	local textTweenInfo = TweenInfo.new(
		tweenInfoDuration,
		announcementService.easingStyle
	)

	if announcementUI then
		cautionClone.Parent = announcementUI.MainFrame
		cautionText.Text = announcementString

		local cautionImg = cautionText.Caution

		positionIcon(cautionImg, cautionText)
		
		cautionText.UIStroke.Thickness = announcementService.uiStrokeThickness
		cautionText.TextTransparency = 1
		cautionText.UIStroke.Transparency = 1
		cautionText.Visible = true
		
		if (not textColor) then
			textColor = Color3.fromRGB(255,255,255)
		end
		
		cautionText.TextColor3 = textColor
		
		local imgIn = TweenService:Create(
			cautionImg, 
			textTweenInfo, 
			{ 
				ImageTransparency = 0 
			}
		)
		
		local running = true

		local swingTweenLeft = TweenService:Create(
			cautionImg,
			TweenInfo.new(0.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), 
			{ Rotation = 15 }
		)
		local swingTweenRight = TweenService:Create(
			cautionImg,
			TweenInfo.new(0.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), 
			{ Rotation = -15 }
		)

		task.spawn(function()
			while running do
				swingTweenRight:Play()
				swingTweenRight.Completed:Wait()
				swingTweenLeft:Play()
				swingTweenLeft.Completed:Wait()
			end
		end)

		local showAnnouncement = TweenService:Create(
			cautionText, 
			textTweenInfo, 
			{
				TextTransparency = 0
			}
		)
		if announcementService.uiStroke then
			local showUiStroke = TweenService:Create(
				cautionText.UIStroke, 
				textTweenInfo, 
				{
					Transparency = 0
				}
			)
			imgIn:Play()
			showUiStroke:Play()
			showAnnouncement:Play()
		end

		task.wait(announcementDuration)
		
		running = false
		
		swingTweenRight:Cancel()
		swingTweenLeft:Cancel()

		local resetRotation = TweenService:Create(
			cautionImg, 
			TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), 
			{ Rotation = 0 }
		)

		resetRotation:Play()

		local imgOut = TweenService:Create(
			cautionImg, 
			textTweenInfo, 
			{ ImageTransparency = 1 }
		)

		local hideAnnouncement = TweenService:Create(
			cautionText,
			textTweenInfo,
			{
				TextTransparency = 1
			}
		)
		if announcementService.uiStroke == true then
			local hideUiStroke = TweenService:Create(
				cautionText.UIStroke,
				textTweenInfo,
				{
					Transparency = 1
				}
			)
			
			imgOut:Play()
			hideUiStroke:Play()
			hideAnnouncement:Play()
		end
		hideAnnouncement.Completed:Wait()
		cautionClone:Destroy()
	end
end

return announcementService
