# power-apps-relative-date-time

Have you ever wanted to display a relative timestamp in your Power Apps? For example, you may want to show how long ago an event occurred rather than the exact date and time.

As of April 2023, Power Apps does not currently include an inbuilt formula for calculating relative dates.

In this blog post, We will see how to achieve this using a little bit of code.

```csharp
With(
    {
        differenceInMinutes: RoundDown(
            DateDiff(
                DateTimeValue(<inputdate>),
                Now(),
                TimeUnit.Minutes
            ),
            0
        )
    },
    If(
        differenceInMinutes <= 5,
        "Now",
        differenceInMinutes < 60,
        differenceInMinutes & " minutes ago",
        differenceInMinutes < 120,
        "1 hour ago",
        differenceInMinutes < 1440,
        RoundDown(
            differenceInMinutes / 60,
            0
        ) & " hours ago",
        differenceInMinutes < 1440 * 2,
        "1 day ago",
        differenceInMinutes < 10080,
        RoundDown(
            differenceInMinutes / 1440,
            0
        ) & " days ago",
        differenceInMinutes < 10080 * 2,
        "1 week ago",
        differenceInMinutes < 43800,
        RoundDown(
            differenceInMinutes / 10080,
            0
        ) & " weeks ago",
        differenceInMinutes < 43800 * 2,
        "1 month ago",
        differenceInMinutes < 525601,
        RoundDown(
            differenceInMinutes / 43800,
            0
        ) & " months ago",
        differenceInMinutes < 525601 * 2,
        "1 year ago",
        RoundDown(
            differenceInMinutes / 525601,
            0
        ) & " years ago"
    )
)

```

This code uses the **'With'** and **'If'** functions to calculate the time difference between the current date and the date of an event. It then formats the result as a relative timestamp. Let's take a closer look at how this works.

The With function is used to define a local variable **'differenceInMinutes'**. This variable holds the time difference in minutes between the date of the event (your input date) and the current date and time (Now()). The DateDiff function is used to calculate the time difference between the two dates in minutes. The **RoundDown** function is used to round down the result to the nearest whole number.

The **If** function is used to format the result as a relative timestamp. The first condition checks if the time difference is less than or equal to 5 minutes. If it is, the result is set to **"Now"** indicating that the event occurred very recently. The other conditions check the time difference and format the result accordingly. For example, if the time difference is between 1 and 2 days, the result is set to "1 day ago". If the time difference is between 1 and 2 weeks, the result is set to "1 week ago" and so on.
