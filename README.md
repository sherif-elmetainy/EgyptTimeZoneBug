# EgyptTimeZoneBug


## Description

In 2024, the Africa/Cairo daylight saving (summer time GMT+3) ended on midnight of Friday, November 1st 2024, where the clock was set back one hour to start the standard time (winter time GMT + 2). 

In the below code, Offset should be 03:00:00 because on October 31 at 9AM it's still summer time. This is the case with the code executed on the server (Server Side Rendering). 
However, when the same code is executed on the client (Web Assembly), the Offset is 02:00:00. 


```razor 
@page "/"

<PageTitle>Home</PageTitle>

<h1>Hello, world!</h1>

<p>The following should output 3:00:00. It does so when rendering from server. However, when client side webassembly code is loaded and takes over, it displays 2:00:00.</p>
@Offset 


@code
{
    private DateTimeOffset Date { get; set; } = new DateTimeOffset(2024, 10, 31, 9, 0, 0, TimeSpan.FromHours(3));
    private TimeSpan Offset { get; set; }

    protected override void OnInitialized()
    {
        var timezone = TimeZoneInfo.FindSystemTimeZoneById("Africa/Cairo");
        Offset = timezone.GetUtcOffset(Date.DateTime);
        base.OnInitialized();
    }
}
```

