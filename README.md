# TimeCalculator
### Scientific Computing with Python, course offered by freecodecamp.org

This function calculates time and day of the week given start time, duration, and current day. Unable to use any python libraries, like **datetime**

Instructions for building this project can be found at https://www.freecodecamp.org/learn/scientific-computing-with-python/scientific-computing-with-python-projects/time-calculator

     def sum_days(d):
      if d == 1:
          return "(next day)"
      elif d > 1:
          return f'({d} days later)'
      return ""


     def add_time(start, duration, day=False):

      total_mins = 1440
      half_mins = 720
      weekdays = [
          "monday",
          "tuesday",
          "wednesday",
          "thursday",
          "friday",
          "saturday",
          "sunday",
      ]

      days_later = 0
      factors = (60, 1) # will multiply factors with hours:minutes to convert to just minutes
      start = start.split(" ")
      am_pm = start[1]
      if am_pm == "AM" and start[0] == 12:
          start[0] = 0
      start_mins = sum(i * j for i, j in zip(map(int, start[0].split(":")), factors)) # this formula converts time to minutes
      if am_pm == "PM" and start[0] != 12:
          start_mins = start_mins + half_mins

      duration = duration.split(":")
      duration_mins = sum(x * y for x, y in zip(map(int, duration), factors))

      sum_mins = start_mins + duration_mins

      # am pm conversions
      if total_mins > sum_mins >= half_mins:
          am_pm = "PM"
      if am_pm == "AM" and sum_mins > total_mins:
          if ((sum_mins / 60) % 12) < 12 and ((sum_mins / 60) % 12) != 0:
              am_pm = "PM"

      if duration_mins:
          if sum_mins >= half_mins:
              mins_left = sum_mins / total_mins
              days_later += int(mins_left)

      new_time = '{:02d}:{:02d}'.format(*divmod(sum_mins, 60))

      # converting mins:mins to hours:mins
      nt = new_time.split(":")
      nt[0] = (int(nt[0]) % 12) 
      for i in range(len(nt)):
          if nt[0] == 0:
              nt[0] = 12
      nt = (':'.join(map(str, nt)))

      new_time = f'{nt} {am_pm}'

      # indexing into weekdays for current day after duration
      if day:
          day = day.strip().lower()
          nday = int((weekdays.index(day) + days_later) % 7)
          today = weekdays[nday]
          new_time += f", {today.capitalize()} {sum_days(days_later)}"

      else:
          new_time = " ".join((new_time, sum_days(days_later)))

      return new_time.strip()
