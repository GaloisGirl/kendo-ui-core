---
title: Views
page_title: Views
description: "Get started with the Scheduler HtmlHelper for {{ site.framework }} and learn how to how to use the default views and create custom views in the widget."
slug: htmlhelpers_scheduler_views_aspnetcore
position: 3
---

# Views

The Scheduler supports different views to display its events.

## Default Views

The {{ site.framework }} Scheduler provides the following built-in views:

- `day`&mdash;Displays the events in a single day.
- `week`&mdash;Displays the events in a whole week.
- `workWeek`&mdash;Displays the events in a work week.
- `month`&mdash;Displays the events in a single month.
- `year`&mdash;Displays the events in a twelve months period.
- `agenda`&mdash;Displays the events from the current date until the next week (seven days).
- `timeline`&mdash;Displays the events for the day in line.
- `timelineWeek`&mdash;Displays the events in a whole week in line.
- `timelineWorkWeek`&mdash;Displays the events in a work week in line.
- `timelineMonth`&mdash;Displays the events for a month in line.

By default, the **Day** and **Week** views are enabled. To enable other views or configure them, use the .Views() option.

> The built-in Scheduler views are designed to render a time-frame that ends on the day it starts. To render views which start on one day and end on another, [build a custom view](#custom-views).

The following example demonstrates how to enable all Scheduler views.

```
	@(Html.Kendo().Scheduler<Kendo.Mvc.Examples.Models.Scheduler.TaskViewModel>()
		.Name("scheduler")
		.Date(new DateTime(2021, 6, 13))
		.StartTime(new DateTime(2021, 6, 13, 7, 00, 00))
		.Height(600)
		.Views(views =>
		{
			views.DayView();
			views.WorkWeekView(workWeekView => workWeekView.Selected(true));
			views.WeekView();
			views.MonthView();
			views.YearView();
			views.AgendaView();
			views.TimelineView();
			views.TimelineWeekView();
			views.TimelineWorkWeekView();
			views.TimelineMonthView();
		})
		.Timezone("Etc/UTC")
		.Resources(resource =>
		{
			resource.Add(m => m.OwnerID)
				.Title("Owner")
				.DataTextField("Text")
				.DataValueField("Value")
				.DataColorField("Color")
				.BindTo(new[] {
					new { Text = "Alex", Value = 1, Color = "#f8a398" } ,
					new { Text = "Bob", Value = 2, Color = "#51a0ed" } ,
					new { Text = "Charlie", Value = 3, Color = "#56ca85" }
				});
		})
		.DataSource(d => d
			.Model(m => {
				m.Id(f => f.TaskID);
				m.Field(f => f.Title).DefaultValue("No title");
				m.Field(f => f.OwnerID).DefaultValue(1);
				m.RecurrenceId(f => f.RecurrenceID);
			})
			.Read("Basic_Usage_Read", "Scheduler")
			.Create("Basic_Usage_Create", "Scheduler")
			.Destroy("Basic_Usage_Destroy", "Scheduler")
			.Update("Basic_Usage_Update", "Scheduler")
			.Filter(filters =>
			{
				filters.Add(model => model.OwnerID).IsEqualTo(1).Or().IsEqualTo(2);
			})
		)
	)
```

## Custom Views

The Scheduler enables you to create custom views which meet the specific project requirements by extending the default `View` classes of the Scheduler. To implement a custom view, extend (inherit from) one of the existing views.

The following source-code files contain the views implementation:

* `kendo.scheduler.view.js`&mdash;Contains the basic logic of the Scheduler views. Each of the other predefined views extends the `kendo.ui.SchedulerView` class.
* `kendo.scheduler.dayview.js`&mdash;Contains the logic which implements the `MultiDayView`. The `MultiDayView` class is further extended to create the `DayView`, the `WeekView`, and the `WorkWeekView`.
* `kendo.scheduler.monthview.js`&mdash;Contains the implementation of the `MonthView` which extends the `SchedulerView`.
* `kendo.scheduler.yearview.js`&mdash;Contains the implementation of the `YearView` which extends the `SchedulerView`.
* `kendo.scheduler.timelineview.js`&mdash;Implements the `TimelineView`, the `TimelineWeekView`, the `TimelineWorkWeekView`, and the `TimelineMonthView`. The `TimelineWeekView`, the `TimelineWorkWeekView`, and the `TimelineMonthView` extend the `TimelineView` class.
* `kendo.scheduler.agendaview.js`&mdash;Implements the `AgendaView` which extends the `SchedulerView`.

You can override each method and property that are defined in the list by extending the respective class. In this way, the functionality and the appearance of the view will be altered by creating the new, custom view. For more information, refer to the `kendo.scheduler.dayview.js` and `kendo.scheduler.timelineview.js` files which contain definitions of views which extend the already defined `MultiDayView` and `TimelineView` views.


{% if site.mvc %}
Implementing a custom views is demonstrated in the the How-To article regarding [Implementing Custom Views](slug % howto_implementcustomviews_scheduleraspnetmvc %). 
{% endif %} 

## See Also

* [Server-Side API](/api/scheduler)
{% if site.core %}
    [Implementing Custom Views UI for ASP.NET MVC](https://docs.telerik.com/aspnet-mvc/html-helpers/scheduling/scheduler/how-to/custom-view) 
{% endif %} 
