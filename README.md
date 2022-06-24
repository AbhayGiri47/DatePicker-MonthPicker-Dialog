# DatePicker-MonthPicker-Dialog
create datepicker dialog and month picker dialog


Date Picker Dialog  

        fun getCalenderDate(context: Context, listener: DateChooseListener) {

            val calender = Calendar.getInstance(TimeZone.getDefault())

            val dialog = DatePickerDialog(context, DatePickerDialog.OnDateSetListener { view, year, month, dayOfMonth ->
                val monthOfYear: Int = month + 1

                var updatedDate: String = ""
                var updatedMonth: String = ""

                if (monthOfYear in 0..9)
                    updatedMonth = "0$monthOfYear"
                else
                    updatedMonth = monthOfYear.toString()

                if (dayOfMonth in 0..9)
                    updatedDate = "0$dayOfMonth"
                else
                    updatedDate = dayOfMonth.toString()

                val selectedDate = "$updatedMonth/$updatedDate/$year"
                listener.onDatePick(selectedDate)

            }, calender.get(Calendar.YEAR), calender.get(Calendar.MONTH), calender.get(Calendar.DAY_OF_MONTH))
            //condition to disable past date
            //dialog.datePicker.minDate = System.currentTimeMillis() - 1000
            dialog.show()
        }
        
        
         interface DateChooseListener {
        fun onDatePick(date: String)
    }
