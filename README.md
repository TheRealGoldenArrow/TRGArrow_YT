function WallCheck(target)
    local vect3 = camera:WorldToScreenPoint(target.Position)
    local unit = camera:ScreenPointToRay(vect3.X, vect3.Y)
    local ray = Ray.new(unit.Origin, unit.Direction * math.min((target.Position - camera.CFrame.p).magnitude, settings.MaxRayLength))
    local ignorelist = {camera}
    local lasthit, hitpos
    if lplayer.Character then ignorelist[#ignorelist+1] = lplayer.Character end

    repeat
        lasthit, hitpos = workspace:FindPartOnRayWithIgnoreList(ray, ignorelist)
        if lasthit then
            if lasthit == target or lasthit.Parent == target.Parent then
                return true
            elseif not settings.IgnoreTransparency or (settings.IgnoreTransparency and (lasthit.Transparency == 0 or lasthit.Transparency == 1)) or lasthit == workspace.Terrain then
                if lasthit.CanCollide then
                    if Debug ~= nil and Debug then
                        coroutine.wrap(function()
                            oldcolor = lasthit.BrickColor
                            oldmat = lasthit.Material
                            oldtransparency = lasthit.Transparency
                            lasthit.Material = Enum.Material.ForceField
                            lasthit.Transparency = 0.1
                            lasthit.BrickColor = BrickColor.new(0,255,255)

                            wait(2.5)
                            lasthit.Transparency = oldtransparency
                            lasthit.Material = oldmat
                            lasthit.BrickColor = oldcolor
                        end)()
                    end
                    return false
                end
            end
            ignorelist[#ignorelist+1] = lasthit
        end
    until not lasthit

    print('Raycast did not hit anything')
    return false
end
