> Texts with under line represents <u>ToolKit</u> <br>
> Texts with bold letters represents **CalcServices**

## basic_sample_get_index_history
1. Flow is same as basic_sample_get_index_composition
2. Sending request to **/index_history** endpoint of CalcServices
3. Calls **backtest.get_index** method of CalcServices
4. Fetches file from "idt_output_data_files_\<environment\>" buckets (idt_output_data_files_sandbox)
5. Returns the content from "\<batch_id\>_index.txt" file

## basic_sample_migrate_stats_collection
1. Send request to **/migration_stats** of CalcServicse
2. The called method fetches the files from "idt_output_data_files_\<environment\>" buckets (idt_output_data_files_sandbox)
3. Files are filtered on this pattern: "*_migration_stats.txt"
4. Fetches "batch_id", "migrate_date", "total_count", "min_date", "max_date" and return the dataframe