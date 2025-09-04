**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeZyt_uR_7j0S34dx-wj29yuX2pIVKpuRa8UZQ5xpNy0-7F8R8IddPHHd5XlqbhPG_BO3D2S1SwtGzjKn4hoTatvDGLzaMineV2GY_FdAC07b1yn0MLYRWUgK43dHe1oAz4d-A_yg?key=YLgeimGOX8daRFwYKh3lPdTg)**
1. Entry 1:
	- Issue: The contrast in the navigation bar between active and inactive tabs is too little to fulfill contrast requirements: https://coolors.co/contrast-checker/e7c6d5-e4afc4. 
	- Violates: Be consistent, internally, and with standards.
	- Severity: (2) Minor issue, users with vision impairments may have some trouble recognizing which tab of the app they are on, but there are titles on each page to help remedy that.
	- Fix: Change the color of the active tab to something with higher contrast.
2. Entry 2:
	- Issue: On the home page, the "edit budget" sub-page is unclear on what the "Spend %" and "Save %" actually accomplish. 
	- Violates: Provide (or remove need for) help/documentation.
	- Severity: (2) Minor issue. It doesn't seem like the "Spend %" and the "Save %" feature is a highlighted part of the app experience. However, this should still be fixed for greater understanding of the app's agenda.
	- Fix: Have a help pop-up appear that relates to the "Spend %" and "Save %". Additionally, have the "$ Saved" on the home page be clear as to how that is calculated via help pop-up or something similar.
3. Entry 3:
	- Issue: When the user edits their budget's save/spend percentage without providing another budget total and has already added expenses to their budget tracker, the "Available Balance" and "Recommended Daily Spending" values change. Negative values fill those metrics which is likely assuming that the new user's budget is $0 even though the user did not edit the "Budget per Month" input field.
	- Violates: Deal with / prevent errors in a positive and helpful manner.
	- Severity: (3) Major usability concern. Every time the user wishes to change their spend/save percentage they must remember what their previous budget was.
	- Fix: Have the previous "Budget per Month" autofill in the input when the user opens the "Edit Budget" page. This way if the user does not interact with that field, the app will recognize that the user wants to keep the previous "Budget per Month".
4. Entry 4:
	- Issue: The radius on the buttons on the Home page and the rest of the pages are different.
	- Violates: Be consistent, internally, and with standards.
	- Severity: (1) Minor cosmetic concern.
	- Fix: Have a standardized style to the buttons ensuring that they remain consistent whenever there is a button.
5. Entry 5:	
	- Issue: It's not certain what happens when the user adds an income amount. There is no feedback to what changes as well as the fact that none of the metrics on the Home page seems to be influenced when the user receives an income.
	- Violates: Provide feedback.
	- Severity: (3) Major usability concern. Logging income is an intrinsic part of a budget app, but this app does not reflect anything related to income other than a "transaction" of "+$\$\$\$"
	- Fix: Somehow intertwine the hard set budget and the incomes. Or have a separate metric that shows the user's true balance rather than what's their balance left from their set budget limit.
6. Entry 6:
	- Issue: It's not clear what happens when the user meets their goal (i.e. no visual feedback other than the filled 100% bar). The user is able to edit the goal to show that they have met their target saved amount but there is no automatic way of reaching that goal without the user manually changing the saved amount. 
	- Violates: Provide feedback.
	- Severity: (2) Minor usability concern. 
	- Fix: Upon goal completion, the goal should be "frozen" and then displayed differently than a goal that is in progress. There should then be a delete/complete goal button easily accessible next to/on the goal itself.