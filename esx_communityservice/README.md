# ESX_COMMUNITYSERVICE

- This is a fork from the old version of ESX_CommunityService but this time it has protected Events so that pesky modders cannot put the whole server in community service.


# Requirements

- es_extended
- skinchanger

# Installation

- Clone this repository
- Add this folder to your [esx] directory
- Import the SQL into your database
- Add `start esx_communityservice` to server.cfg
- And you're done!

# How to add to policejob menu.

Example in `esx_policejob: client/main.lua`:

```lua
-- ADDITION [1]
{label = _U('fine'),			value = 'fine'},
{label = _U('unpaid_bills'),	value = 'unpaid_bills'},
-- add code below (don't forget to add ',' before new row)
{label = "Community Service",	value = 'communityservice'}


-- ADDITION [2]
elseif action == 'unpaid_bills' then
	OpenUnpaidBillsMenu(closestPlayer)
-- add code below
elseif action == 'communityservice' then
	SendToCommunityService(GetPlayerServerId(closestPlayer))
end


-- ADDITION [3]
-- add this function
function SendToCommunityService(player)
	ESX.UI.Menu.Open('dialog', GetCurrentResourceName(), 'Community Service Menu', {
		title = "Community Service Menu",
	}, function (data2, menu)
		local community_services_count = tonumber(data2.value)
		
		if community_services_count == nil then
			ESX.ShowNotification('Invalid services count.')
		else
			TriggerServerEvent("esx_communityservice:sendToCommunityService", player, community_services_count)
			menu.close()
		end
	end, function (data2, menu)
		menu.close()
	end)
end
