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
    
    
    
   
Month Picker Dialog
    
    filterBinding.edtYear.setOnClickListener {
                /* dateFlag = 2
                 CommonUtils.getCalenderDate(activity!!, this)*/
                val pickerDialog = MonthYearPickerDialog()
                pickerDialog.setListener { view, year, month, dayOfMonth ->
                    this.yearAnalyst = year
                    filterBinding.edtYear.setText(year.toString())
                }
            pickerDialog.show(fragmentManager, "MonthYearPickerDialog")
        }
        
  
  public class MonthYearPickerDialog extends DialogFragment {
    private static final int MAX_YEAR = 2099;
    private DatePickerDialog.OnDateSetListener listener;

    public void setListener(DatePickerDialog.OnDateSetListener listener) {
        this.listener = listener;
    }

    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
        AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
        // Get the layout inflater
        LayoutInflater inflater = getActivity().getLayoutInflater();

        Calendar cal = Calendar.getInstance();

        View dialog = inflater.inflate(R.layout.month_year_picker, null);
        final NumberPicker monthPicker = (NumberPicker) dialog.findViewById(R.id.picker_month);
        final NumberPicker yearPicker = (NumberPicker) dialog.findViewById(R.id.picker_year);

        monthPicker.setMinValue(1);
        monthPicker.setMaxValue(12);

        monthPicker.setDisplayedValues(new String[]{"January","February","March","April","May","June","July",
                "August","September","October","November","December"});


        monthPicker.setValue(cal.get(Calendar.MONTH) + 1);

        int year = cal.get(Calendar.YEAR);
        yearPicker.setMinValue(year-50);
        yearPicker.setMaxValue(year+50);
        yearPicker.setWrapSelectorWheel(false);
        yearPicker.setValue(year);

        builder.setView(dialog)
                // Add action buttons
                .setPositiveButton("OK", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int id) {
                        listener.onDateSet(null, yearPicker.getValue(), monthPicker.getValue(), 0);
                    }
                })
                .setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
                    public void onClick(DialogInterface dialog, int id) {
                        MonthYearPickerDialog.this.getDialog().cancel();
                    }
                });
        return builder.create();
    }
}





Date Parser 

class DateParser {

    companion object{
        @SuppressLint("SimpleDateFormat")
        fun parseDateAppFormatFrom(dateString : String, iDateFormat:String = Constant.SYSTEM_DATE_FORMAT):String{

            return try {
                val iSdf = SimpleDateFormat(iDateFormat)
                val odf = SimpleDateFormat(Constant.COMMON_DATE_FORMAT)
                val formattedDate = iSdf.parse(dateString)
                val dateInString = odf.format(formattedDate)
                dateInString
            }catch (e:Exception){
                dateString
            }
        }

        @SuppressLint("SimpleDateFormat")
        fun parseDateAppFormatTo(dateString : String, iDateFormat:String = Constant.SYSTEM_DATE_FORMAT):String{

            return try {
                val iSdf = SimpleDateFormat(Constant.COMMON_DATE_FORMAT)
                val odf = SimpleDateFormat(iDateFormat)
                val formattedDate = iSdf.parse(dateString)
                val dateInString = odf.format(formattedDate)
                dateInString
            }catch (e:Exception){
                dateString
            }
        }

        @SuppressLint("SimpleDateFormat")
        fun parseDateToYearFirst(dateString : String, ipDateFormat:String,opDateFormat: String):String{

            return try {
                val iSdf = SimpleDateFormat(ipDateFormat)
                val odf = SimpleDateFormat(opDateFormat)
                val formattedDate = iSdf.parse(dateString)
                val dateInString = odf.format(formattedDate)
                dateInString
                /*val sdfInput = SimpleDateFormat("MM/dd/yyyy")//getting dateformat
                  val sdfInput1 = SimpleDateFormat("yyyy-MM-dd")

                  val apiDate = sdfInput.parse(date)
                  val dateInString = sdfInput1.format(apiDate)
                 */
            }catch (e:Exception){
                dateString
            }
        }

    }

}
