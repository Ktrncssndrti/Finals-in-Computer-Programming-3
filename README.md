using System;

class Program
{
    static void Main(string[] args)
    {
        // Get time-in and time-out from user input
        DateTime timeIn = GetDateTimeFromUserInput("Enter time-in AM/PM: ");
        DateTime timeOut = GetDateTimeFromUserInput("Enter time-out AM/PM: ");

        // Calculate and output the total working hours
        TimeSpan totalWorkingHours = CalculateTotalWorkingHours(timeIn, timeOut);
        Console.WriteLine("Total working hours:{0} ", totalWorkingHours);

        // Calculate and output the total regular hours
        TimeSpan totalRegularHours = CalculateTotalRegularHours(timeIn, timeOut);
        Console.WriteLine("Total regular hours: {0} ", totalRegularHours);

        // Calculate and output the late time
        TimeSpan lateTime = CalculateLateTime(timeIn);
        Console.WriteLine("Late time: {0}", lateTime);

        // Calculate and output the undertime hours
        TimeSpan undertimeHours = CalculateUndertimeHours(totalRegularHours);
        Console.WriteLine("Undertime hours: {0}", undertimeHours);

        // Calculate and output the overtime hours
        TimeSpan overtimeHours = CalculateOvertimeHours(totalWorkingHours);
        Console.WriteLine("Overtime hours: {0}", overtimeHours);
    }

    static DateTime GetDateTimeFromUserInput(string message)
    {
        Console.Write(message);
        return DateTime.Parse(Console.ReadLine());
    }

    static TimeSpan CalculateTotalWorkingHours(DateTime timeIn, DateTime timeOut)
    {
        TimeSpan lunchBreak = TimeSpan.FromHours(1);
        TimeSpan totalWorkingHours = timeOut - timeIn - lunchBreak;
        return totalWorkingHours;
    }

    static TimeSpan CalculateTotalRegularHours(DateTime timeIn, DateTime timeOut)
    {
        TimeSpan totalWorkingHours = CalculateTotalWorkingHours(timeIn, timeOut);
        TimeSpan regularWorkingHours = TimeSpan.FromHours(8);
        TimeSpan totalRegularHours = totalWorkingHours < regularWorkingHours ? totalWorkingHours : regularWorkingHours;
        return totalRegularHours;
    }

    static TimeSpan CalculateLateTime(DateTime timeIn)
    {
        TimeSpan workingHoursStart = TimeSpan.FromHours(8);
        TimeSpan gracePeriod = TimeSpan.FromMinutes(30);
        TimeSpan lateTime = TimeSpan.Zero;
        if (timeIn.TimeOfDay > workingHoursStart.Add(gracePeriod))
        {
            lateTime = timeIn.TimeOfDay - workingHoursStart.Add(gracePeriod);
        }
        return lateTime;
    }

    static TimeSpan CalculateUndertimeHours(TimeSpan totalRegularHours)
    {
        TimeSpan regularWorkingHours = TimeSpan.FromHours(8);
        TimeSpan undertimeHours = totalRegularHours < regularWorkingHours ? regularWorkingHours - totalRegularHours : TimeSpan.Zero;
        return undertimeHours;
    }

    static TimeSpan CalculateOvertimeHours(TimeSpan totalWorkingHours)
    {
        TimeSpan regularWorkingHours = TimeSpan.FromHours(8);
        TimeSpan overtimeHours = totalWorkingHours > regularWorkingHours ? totalWorkingHours - regularWorkingHours : TimeSpan.Zero;
        return overtimeHours;
    }
}
