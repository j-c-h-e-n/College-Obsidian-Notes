![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc_rp2qDoIFEZYkZkEbjVOjOrLu_usMsb97qYDJOc7sNsB_Mj738rKf24MwBbxJ6gv3doJlJ3ikN83zoWWBcNMjw9py7daupCPz16MDGp8YMp9OYvT_zILxcso9Ip_2r1K08B-TIg?key=YLgeimGOX8daRFwYKh3lPdTg)

Entry 1:
- Issue: Overall text sizes are too small across all pages
- Violates: Be consistent, internally and with standards.
- Severity: (2) Minor concern but will result in users straining to read text.
- Fix: Increase text sizes across the entire app. Most especially sub-text.
Entry 2:
- Issue: Color contrast regarding text, navigation bar, and secondary element colors are too minimal to confidently tell active elements.
- Violates: Be consistent, internally and with standards.
- Severity: (3) Major usability concern 
- Fix: 
	- To fix the text, have the fonts be greater weights or increase their sizes.
	- The navigation bar should have a different highlight color or have a filled colored radius around the icons to highlight which tab is active.
	- Change the secondary color elements to a higher contrast against the white background while still maintaining contrast with the primary dark blue.
Entry 3:
- Issue: When adding budgets and goals there are additional cards that are labeled as NaN/Unnamed which do not have any associated information.
- Violates: Deal with/prevent errors in a positive and helpful manner.
- Severity: (2) Minor usability concern. This definitely clutters up the user's view but with some difficulty, they can be removed (see Entry 5).
- Fix: Prevent NaN and Unnamed elements to be added when creating budgets and goals.
Entry 4:
- Issue: Users may want to view their budget allocations and goals but they have to traverse through multiple pages to view them. There is no clear way of navigating to those pages. Currently budget allocations requires the user to press "Manage Budget" in the navigation, then press the "Adjust" button for "Adjust Budget" underneath "Budget Management". Then inside "Adjust Budget" there is a button to press to "View Budget Allocations". With goals, users navigate the same way as before to the "Manage Budget" page. Then users have to press "Set Goal" and inside that page there is a "View My Goals".
- Violates: Provide Shortcuts
- Severity: (3) Major usability concern. 
- Fix: Provide a direct path to viewing budgets and goals in the "Manage Budget" screen. A clearer path would be instead of "Adjust Budget" and "Set Goal" the parent tab should instead be "View Budget" and "View Goals". Then the user should be allowed to adjust and set.
Entry 5:
- Issue: A significant portion of the buttons/functionality do not work. 
	- Home page: "Manage Budget"; 
	- Reports page: "This Week" time frame filtering, "Week View/Month View" in the first and second sub-tabs, "Adjust Budget" in the second sub-tab, "Active/Completed" filtering in the third sub-tab.
	- Manage Budget page:
		- Recurring Transactions: every single interactable element does not work.
		- Bills: Filtering does not work. Editing each item does not work. Marking items as paid or deleting items do not actually update when the page is exited and re-entered.
	- Settings page:
		- Everything except the Dark Mode and High Contrast toggles do not work.
			- High Contrast only applies an effect in light mode.
- Violates: Provide feedback
- Severity: (4) Catastrophic concern where users try to use buttons but nothing happens. Will deteriorate user opinion, leading to abandonment of the app.
- Fix: Provide some notification that the button/feature has not been implemented yet. So many features do not work, including crucial core features, so they should be pruned or implemented.