demo-ps


source="/home/tmp/prod/U/sampledata.csv" host="devsplunk" sourcetype="csv" | eval ServiceDescription = case(ID_SVC = "1","Transaction Scrub and Geocode",ID_SVC = "2","Transaction Scrub and Geocode with limits", ID_SVC = "3","smallbatch",ID_SVC = "4","Loan limits reference data",ID_SVC = "5","EPMS") | rex field=DI_SVC_END "(?<hour>\d{1,2}):(?<min>\d{2}).(?<sec>\d{1})"  | eval duration_s1 = (hour*3600)+(min*60)+sec  |  rex field=DI_SVC_STARTD "(?<hour1>\d{1,2}):(?<min1>\d{2}).(?<sec1>\d{1})" | eval duration_s2 = (hour1*3600)+(min1*60)+sec1 | eval Total_processing_time_Sec=(duration_s1-duration_s2) | stats sum(CNT_RESCS_RETD) as Total_Address_processed sum(Total_processing_time_Sec) as Total_processing_time_Sec by  ServiceDescription,ID_SVC | eval Throughput_per_sec=Total_Address_processed/Total_processing_time_Sec | eval Throughput_per_hour=Throughput_per_sec/3600 | eval Average_processing_time_per_address=Total_processing_time_Sec/Total_Address_processed | table ID_SVC,ServiceDescription,Total_Address_processed,Total_processing_time_Sec,Throughput_per_sec,Throughput_per_hour,Average_processing_time_per_address
  
  
  *************
  
  
  source="/home/tmp/prod/U/sampledata.csv" host="devsplunk" sourcetype="csv" | eval ServiceDescription = case(ID_SVC = "1","Transaction Scrub and Geocode",ID_SVC = "2","Transaction Scrub and Geocode with limits", ID_SVC = "3","smallbatch",ID_SVC = "4","Loan limits reference data",ID_SVC = "5","EPMS") | rex field=DI_SVC_END "(?<hour>\d{1,2}):(?<min>\d{2}).(?<sec>\d{1})"  | eval duration_s1 = (hour*3600)+(min*60)+sec  |  rex field=DI_SVC_STARTD "(?<hour1>\d{1,2}):(?<min1>\d{2}).(?<sec1>\d{1})" | eval duration_s2 = (hour1*3600)+(min1*60)+sec1 | eval Total_processing_time_Sec=(duration_s1-duration_s2) | table ID_SVC, ServiceDescription, DI_SVC_STARTD, DI_SVC_END,Total_processing_time_Sec | where Total_processing_time_Sec>300
