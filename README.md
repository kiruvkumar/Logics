# Logics
1.	Web_reg_save_param_ex()
If we get dynamic value in cookies : check the id of cookie. Find out the next id. Place the correlation function before the next id call in script :
web_reg_save_param_ex(
           "ParamName=THDSessionID",
	       "LB=sessionID=",
	           "RB=;Version=1",
        	   SEARCH_FILTERS,
	           "Scope=Headers",
	           LAST);
2.	Comparison - strcmp(): 
	if((strcmp(lr_eval_string("{Response2}"),"true") == 0),(strcmp(lr_eval_string("{Response3}"),"true") == 0))
3.	Cache and Cookie:
	web_cache_cleanup();
	web_cleanup_cookies();
	web_set_max_html_param_len("99999");
4.	Think Time: 
		 lr_think_time(atoi(lr_eval_string("{Thinktime}")));
5.	Do While:
	do {
        	lr_output_message( "DO WHILE Loop number %d", i);
		i++;
		} 
	while (i <= 10) ;


6.	Replenishment Application general correlation:

	web_reg_save_param("UploadID", "LB=Upload id is ", "RB=\"", "NotFound=Warning", LAST);
	web_reg_save_param("ResponseMsg", "LB=", "RB=", "NotFound=Warning", LAST);
	lr_start_transaction("ReplenishmentLifeCycle_02_SalesCopy_UploadFile132kRows_03_UploadFile");
	<Call which is having the upload request>
	if(strstr(lr_eval_string("{ResponseMsg}"),"File Uploaded Successfully"))
	{
        if(strcmp((lr_eval_string("{UploadID}"),"")) == 0)
			{
			lr_error_message("No Upload ID generated");
			lr_end_transaction("ReplenishmentLifeCycle_02_SalesCopy_UploadFile132kRows_03_UploadFile",LR_FAIL);
            }
	else 
		{
        	lr_output_message("132k Upload ID: %s", lr_eval_string("{UploadID}"));
			lr_end_transaction("ReplenishmentLifeCycle_02_SalesCopy_UploadFile132kRows_03_UploadFile",LR_AUTO);
		}
    }
	else
	{
		lr_error_message("Failed to upload 132k file, Response: %s", lr_eval_string("{ResponseMsg}"));
		lr_end_transaction("ReplenishmentLifeCycle_02_SalesCopy_UploadFile132kRows_03_UploadFile",LR_FAIL);
	}
7.	Comparison:
lr_output_message("str %d",strcmp(lr_eval_string("{ResponseMessage}"),"newSkuNumber"));
if(strcmp(lr_eval_string("{ResponseMessage}"),"newSkuNumber")<0)
{
	lr_output_message(lr_eval_string("No Records Found for the SKU: {NewSKUNmbr}"));
}
	lr_end_transaction("NewSKUSeacrch",LR_AUTO);
8.	Sprintf:
sprintf(Buffer, "{ProjectManager_%d}", atoi(lr_eval_string("{ProjectManager_count}")));
    	lr_save_string(lr_eval_string(Buffer), "ProjectManagerName");
	sprintf(Buffer, "{ProjectName_%d}", atoi(lr_eval_string("{ProjectName_count}")));
    	lr_save_string(lr_eval_string(Buffer), "ProjectName");
	sprintf(Buffer, "{ProjectVP_%d}", atoi(lr_eval_string("{ProjectVP_count}")));
    	lr_save_string(lr_eval_string(Buffer), "ProjectVPName");
9.	File Download Check:
After the web request call:
 i = web_get_int_property(HTTP_INFO_DOWNLOAD_SIZE); 
         if(i > 0) 
    { 
		lr_end_transaction(lr_eval_string("PMO_BasicInformation_EditSave_009_EditSaveInfo-AttachCBASize-{FileSize}"), LR_AUTO);
    } 
    else
    {
		lr_error_message(lr_eval_string("Attach Excel Sheet is not sucessful for the user: {Username} for the iteration: {Iteration_Num}"));
        lr_end_transaction("PMO_BasicInformation_EditSave_009_EditSaveInfo-AttachCBASize-{FileSize}", LR_FAIL); 
	}
10.	Correlation - Concat:
	int i,count,j;
	char tempStr[1024],buffer[1024];
 // For loop is to acknowledge and dismiss all the alerts listed
	for(i=1;i<=atoi(lr_eval_string("{AlertNmbr}"));i++)
		{
		count=atoi(lr_eval_string("{AlertNmbr}"));
		//lr_eval_string("{strRequest}") == NULL;
		
                strcat(tempStr,"");
				for (j=1;j<=count;j++) 
                {
				strcpy(tempStr,"");
                                strcat( tempStr,"{\\\"vndrNtfyConfgId\\\":");
                                sprintf(buffer,"{vndrNtfyConfgId_%d}",j);
                                strcat( tempStr,lr_eval_string(buffer) );
                                strcat( tempStr,",\\\"currentStatusTypeCode\\\":1,\\\"futureStatusTypeCode\\\":2,\\\"systemUserId\\\":\\\"");
                                sprintf(buffer,"{systemUserId_%d}",j);
                                strcat( tempStr,lr_eval_string(buffer) );
                                strcat( tempStr,"\\\",\\\"extnlBusPrtnrIdList\\\":[");
				sprintf(buffer,"{PrtnrID_%d}",j);
				strcat( tempStr,lr_eval_string(buffer) );
                                strcat( tempStr,"],\\\"vndrNtfyLvlCd\\\":\\\"");
				sprintf(buffer,"{vndrNtfyLvlCd_%d}",j);
				strcat( tempStr,lr_eval_string(buffer) );
                                strcat( tempStr,"\\\"}");
				lr_save_string( tempStr,"strRequest");
                                										
}
11.	Depot Direct Error handling:
if (strcmp(lr_eval_string("{ResultCode}"),"0") != 0) {
		lr_output_message("Message: %s",lr_eval_string("{ResultMessage}"));
		if (strcmp(lr_eval_string("{ResultMessage}"),"No Models Found") != 0) {
			return 1;
		}
	}
12.	Correlation with DIG\ALNUM:
web_reg_save_param(“DynamicCapture”, “LB/IC/DIG=a####b\=”, “RB/IC=rb”, LAST);
web_reg_save_param(“DynamicCapture”, “LB/ALNUM=a^b\=”, “RB/IC=rb”, LAST);
LB\RE:
LB\RE=[0-9]* means may or may not take two digits. but it is not appliacable for four digit values
this support only in _ex functions with full ordinal naming




13.	TimeOut Set
web_set_sockets_option("IGNORE_PREMATURE_SHUTDOWN", "1");
web_set_timeout( STEP, "600");
  web_set_timeout( RECEIVE , "600");
  web_set_timeout( CONNECT , "600");
web_set_max_html_param_len("1048576");
web_set_user("9715", "9715", "mycd-qp.homedepot.com");
14.	Microstrategy Do.... While............:
do
					{
					web_reg_find("Text=Processing request",
						"SaveCount=Count",
						"Search=All",
						LAST);
	<Call which need to be repeated>
		}while((atoi(lr_eval_string("{Count}")))>0);

15.	Checking the length of correlated parameter:
 do
{
  web_reg_save_param("SWEACn", "LB=Frame&SWEACn=", "RB=&SWEC", LAST);
  web_custom_request("start.swe_2", 
    "URL=https://sfiqaint.homedepot.com/prmportal_enu_sfi/start.swe?SWECmd=Login&SWEPL=1&_sn={snID}&SWETS=&SWEHo=sfiqaint.homedepot.com", 
    "Method=GET", 
    "Resource=0", 
    "RecContentType=text/html", 
    "Referer=", 
    "Snapshot=t37.inf", 
    "Mode=HTTP", 
    LAST);
}while(strlen(lr_eval_string("{SWEACn}"))>4);
16.	randomize:

int count, randNo;
 char buffer[100]; 

 count=atoi(lr_eval_string("{department_count}"));
 randNo=(rand()%count)+1;
 sprintf(buffer,"{department_%d}",randNo);
 lr_save_string(lr_eval_string(buffer),"deptmt");
 that determines the range of the randome number from a count: 


web_reg_save_param("search_param", "LB=<p><A HREF=", "RB=>", "ORD=All"); 
... search 
rand_selection = ( rand() % atoi(lr_eval_string("{search_param_count}")) + 1); 
The resulting new parameter (rand_selection) is used as the suffix to obtain the value found by the search: 


sprintf(my_new_parameter, "{search_param_%d}", rand_selection); 


17.	LR Exit:

lr_exit(LR_EXIT_ITERATION_AND_CONTINUE, LR_FAIL);

18.	Memory % calculation

Depending on your test case requirements you have to select the below mentioned methods – 
1.       (Total Memory - Average free memory) *  100 / Total memory
*  If the SLA doesn’t meet proceed for this method
2.       (Memory free just before starting of test – Minimum free memory during test)*100 / Total Memory.
If the minimum memory usage of the server is above 90%, we have to take the difference between Average and Minimum memory  usage for the results
Avg 98.467  - Min 97
Memory used is 1.467%

19.	Print To Notepad Logic:
	char newvalue[15];
	long pfile;
	int a;
Action()
{    //Drive: C (Choose any drive in your machine)
    //Folder: SKU_List (Create any folder of your choice)
    //File: SKU_List_25July2012.txt (Define file name where sKU IDs will be exported)
    //Modiy the below path according to your selection made
	char *filename = "C:\\SKU_List\\SKU_List_25July2012.txt";
	-------------------------------------
web_reg_save_param( "sku", "LB=skuNumber>", "RB=<", "Ord=1", "IgnoreRedirections=Yes", "Search=Body", "RelFrameId=1", LAST );
	lr_output_message("SKU ID = %s",lr_eval_string("{sku}"));
	sprintf(newvalue,"%s",lr_eval_string("{sku}"));
		pfile = fopen( filename,"a+" );
		if(pfile == NULL)
				{
					  printf("Error in opening the filename - %s ",filename );
				}	
	fputs(newvalue,pfile );
			fputs( "\n",pfile );
			fclose( pfile );
	web_cleanup_cookies();
	web_cache_cleanup();
}

20.	Changin the format of Timestamp:
lr_save_datetime("%a %b %d %H:%M:%S EDT %Y", DATE_NOW, "TimeStamp1");
web_convert_param("TimeStamp1", "SourceEncoding=PLAIN", "TargetEncoding=URL",LAST );
Output:
TimeStamp1 = Sun Sep 15 02:07:02 EDT 2013".
TimeStamp1 = Sun+Sep+15+02%3A07%3A02+EDT+2013".


21.	To  pass records in multiple page to request.
web_reg_save_param("RecordCount", "LB=fromIndex\":0,\"toIndex\":500,\"totalRecordCount\":", "RB=,\"paginationLimit", LAST);
//timer to remove the response time for other pages except first page
	str_timer_ped= lr_start_timer();
	//Logic
	fromrange=500;
	torange=1000;
	if (atol(lr_eval_string("{RecordCount}")) <= 500)
	{
		loopcount_float=0;
	}
	else
	{
		if(atol(lr_eval_string("{RecordCount}"))%500==0)
		{
			loopcount_float = atol(lr_eval_string("{RecordCount}"))/500 - 1;
		}
		else
		{
			loopcount_float = atol(lr_eval_string("{RecordCount}"))/500;
		}
			
	}
	loopcount_int = loopcount_float;
		//lr_output_message("loopcount=%d", loopcount);
		for (i=1; i <= loopcount_int; i++)
	{
		//lr_output_message("FromRange=%d", fromrange);
		//lr_output_message("FromRange=%d", torange);
		lr_save_int(fromrange, "fromrange"); 
		lr_save_int(torange, "torange");
		//lr_output_message(lr_eval_string("{fromrange}"));
		//lr_output_message(lr_eval_string("{torange}"));
		web_submit_data("betasearch", 
		"Action=https://webapps-qp.homedepot.com/PrcEngineWS/rs/request/grid/betasearch", 
		"Method=POST", 
		"RecContentType=application/json", 
		"Referer=https://webapps-qp.homedepot.com/PricingEngine/", 
		"Snapshot=t60.inf", 
		"Mode=HTTP", 
		ITEMDATA, 
		"Name=versionNumber", "Value=1", ENDITEM, 
		"Name=langCode", "Value=en_US", ENDITEM, 
		"Name=jsonData", "Value={\"merchHierarchyList\":[{\"subdeptData\":\"{Login_SubDpt}\"}],\"byoList\":[],\"marketList\":[],\"sku\":[],\"competitor\":[],\"priority\":[{\"code\":\"1\",\"desc\":\"\"},{\"code\":\"2\",\"desc\":\"\"},{\"code\":\"3\",\"desc\":\"\"}],\"matchType\":{\"code\":\"0\",\"desc\":\"All\"},\"skuStatus\":[{\"code\":\"100\",\"desc\":\"\"},{\"code\":\"200\",\"desc\":\"\"},{\"code\":\"300\",\"desc\":\"\"},{\"code\":\"400\",\"desc\":\"\"}],\"workSheetGeneratedInclude\":false,\"feedbackInclude\"" 
	":false,\"isRestricted\":true,\"userId\":\"{username}\",\"fromIndex\":{fromrange},\"toIndex\":{torange},\"totalRecordCount\":{RecordCount}}", ENDITEM, 
		LAST);
		fromrange = fromrange + 500;
		torange = torange + 500;
		//lr_output_message("FromRange=%d", fromrange);
		//lr_output_message("FromRange=%d", torange);
	}

	time_elapsed_ped = lr_end_timer(str_timer_ped);
	wasteTime_ped = time_elapsed_ped*1000; 
	lr_wasted_time(wasteTime_ped);


22.	CDA Project: Convert to URL

 If we are not able to convert the correlated value to url. We will be using the below method.

Please find the below sample script

Global Declaration

#ifndef _GLOBALS_H 
#define _GLOBALS_H

//--------------------------------------------------------------------
// Include Files
#include "lrun.h"
#include "web_api.h"
#include "lrw_custom_body.h"

//--------------------------------------------------------------------
// Global Variables
// 
 	char sIn[];
	char sOut[200];
	char url_agentname[100];
	int time_elapsed;
    merc_timer_handle_t timer;
	int result;

	char param_min[] ="0";
	char H2[] = "0";

    /*char newvalue[15];
	 char newvalue1[100];
	 char newvalue2[300];
	long pfile;
	//char *filename = "\\\\wsprap026\\h$\\ReducedTag\\TruckloadUser.txt";
    //H:\CDA\Logs
    //C:\CDA\Logs

	char *filename = "H:\\CDA\\Logs\\CDAUser.txt";*/

#endif // _GLOBALS_H


Action - PlaintoURL
	char * plaintourl(const char *sIn, char *sOut)
	{   
		 int i;
		 char cCurChar;
		 char sCurStr[4] = {0}; 
		 sOut[0] = '\0';

		for (i = 0; cCurChar = sIn[i]; i++)
		 {        // if this is a digit or an alphabetic letter  

			  if (isdigit(cCurChar) || isalpha(cCurChar))
				{            // then write the current character "as is" 
				   sprintf(sCurStr, "%c", cCurChar);
				}
			 else if((cCurChar=='_') || (cCurChar=='!')){   //To handle the exception for "_" and "!"
                  sprintf(sCurStr, "%c", cCurChar);
                }
			 else if((cCurChar=='\\')){  	
                  sprintf(sCurStr, "","");
			   }
			else   
			  {            // else convert it to hex-form. "_" -> "%5F"    
				   sprintf(sCurStr, "%%%X", cCurChar);  
			  }        // append current item to the output string

				strcat(sOut, sCurStr);

		 }  
		return sOut;

		}




Action
z_WorderSearch()
{


    lr_start_transaction("CDA_JIA_WOrder_03_WOrderSearch");

	web_add_auto_header("Accept", 
		"*/*");

	web_add_header("Cache-Control", 
		"no-cache");

    														
	web_add_auto_header("x-requested-with", 
		"XMLHttpRequest");

	lr_continue_on_error(1);

	//Status\":\"Complete\",\"LOB\":\"GM\",\"Email\"
	web_reg_save_param("WParam_Response","LB=LOB\\\":\\\"","RB=\\\",\\\"Email","Notfound=warning",LAST); 
																		
   //web_reg_save_param("WParam_Response","LB=OrderId\\\":\\\"","RB=\\\"","Notfound=warning",LAST); 

	
	web_submit_data("ItemAndOrderSearch.json", 
		"Action={url}/CDA/ItemAndOrderSearch.json", 
		"Method=POST", 
		"TargetFrame=", 
		"RecContentType=application/json", 
		"Referer={url}/CDA/com/jacada/cda/itemAndOrderSearch/ItemAndOrderSearch.jsp", 
		"Snapshot=t26.inf", 
		"Mode=HTML", 
		ITEMDATA, 
		"Name=method", "Value=searchOrders", ENDITEM, 
		"Name=orderNo", "Value={WOrderSearch}", ENDITEM, 
		"Name=email", "Value=", ENDITEM, 
		"Name=phone", "Value=", ENDITEM, 
		"Name=zip", "Value=", ENDITEM, 
		"Name=lastName", "Value=", ENDITEM, 
		"Name=firstName", "Value=", ENDITEM, 
		LAST);

	lr_continue_on_error(0);

	if((strcmp(lr_eval_string("{WParam_Response}"),"GM") == 0))
	{
        lr_end_transaction("CDA_JIA_WOrder_03_WOrderSearch",LR_AUTO);
    }
	else
	{
		lr_error_message("W order %s search failed for the user:%s",lr_eval_string("{WOrderSearch}"),lr_eval_string("{uname}"));
        lr_end_transaction("CDA_JIA_WOrder_03_WOrderSearch",LR_FAIL);
    }



	lr_think_time(atoi(lr_eval_string("{Thinktime}")));
	return 0;
}

VuserEnd:

vuser_end()
{
   char Hours[20];
   char Minutes[20];
   char Seconds[20];
   char b[20];
   char c[20];
   char d[20];
   int a=0,a1=0,a2=0;
   char zero[20];
   int h,m,s;

   int Param_Hlen,Param_Mlen,Param_Slen;
   char S2[] = "0";


	time_elapsed = lr_end_timer(timer);

	lr_output_message("Duration of web_reg_save_param = %d", time_elapsed);

    lr_save_int(time_elapsed,"abc");

	lr_output_message("%s",lr_eval_string("{abc}"));

	if(atoi(lr_eval_string("{abc}"))>0)
	{
		h=(atoi(lr_eval_string("{abc}")))/3600;
		m=(atoi(lr_eval_string("{abc}")))/60;
		s=(atoi(lr_eval_string("{abc}")))%60;

		
		lr_save_int(h,"H1");   // Format Hour value if length < 2
		Param_Hlen =strlen(lr_eval_string("{H1}"));
        if(Param_Hlen <2){
			strcat(H2,lr_eval_string("{H1}"));
			lr_save_string(H2,"H3");
                 		}
        else
		{
			lr_save_string(lr_eval_string("{H1}"),"H3");
		}

		lr_save_int(s,"S1");  // Format Seconds value if length < 2
		Param_Slen =strlen(lr_eval_string("{S1}"));
		if(Param_Slen <2){
				strcat(S2,lr_eval_string("{S1}"));
				lr_save_string(S2,"S3");
				 }
		else {
			lr_save_string(lr_eval_string("{S1}"),"S3");
			 }

		lr_save_int(m,"M1");   // Format Minutes value if length < 2
		Param_Mlen =strlen(lr_eval_string("{M1}"));
        if(Param_Mlen <2) {
				lr_output_message("%s",param_min);
				strcat(param_min,lr_eval_string("{M1}"));
				lr_save_string(param_min,"M3");
				 }
		else {
			lr_save_string(lr_eval_string("{M1}"),"M3");
		     }

 
		}

		else
		{
			lr_save_string("55","S1");
		}

	plaintourl(lr_eval_string("{Checkstart}"),sOut);
	lr_save_string(sOut,"Checkstart_url"); 

// here we are calling the defined function

	lr_start_transaction("CDA_JIA_WOrder_04_UnCheckOfline");

	web_submit_data("CustomerQuickView.json", 
		"Action={url}/CDA/CustomerQuickView.json", 
		"Method=POST", 
		"TargetFrame=", 
		"RecContentType=application/json", 
		"Referer={url}/CDA/com/jacada/cda/customerQuickView/CustomerQuickView.jsp", 
		"Snapshot=t32.inf", 
		"Mode=HTML", 
		ITEMDATA, 
		"Name=method", "Value=auditCustomerInfo", ENDITEM, 
		"Name=callID", "Value=", ENDITEM, 
		"Name=customerName", "Value=", ENDITEM, 
		"Name=phone", "Value=", ENDITEM, 
		"Name=email", "Value=", ENDITEM, 
		"Name=billingAddress", "Value=&nbsp;", ENDITEM, 
		"Name=shippingAddress", "Value=&nbsp;", ENDITEM, 
		"Name=orderNumber", "Value=&nbsp;", ENDITEM, 
		"Name=orderStatus", "Value=&nbsp;", ENDITEM, 
		"Name=orderTotal", "Value=&nbsp;", ENDITEM, 
		"Name=creationDate", "Value=&nbsp;", ENDITEM, 
		"Name=storeNumber", "Value=", ENDITEM, 
		"Name=callStart", "Value={CallStart}", ENDITEM, 
		"Name=callDuration", "Value={H3}:{M3}:{S3}", ENDITEM,
				EXTRARES, 
		"Url=USER/resources/images/button_Grey_Hover.png", "Referer={url}/CDA/SYSTEM/portlets/logout/LogoutController.jpf", ENDITEM, 
		LAST);


	web_custom_request("Notes.json", 
		"URL={url}/CDA/Notes.json", 
		"Method=POST", 
		"TargetFrame=", 
		"Resource=0", 
		"RecContentType=application/json", 
		"Referer={url}/CDA/com/jacada/cda/notes/Notes.jsp", 
		"Snapshot=t33.inf", 
		"Mode=HTML", 
		"EncType=application/x-www-form-urlencoded; charset=UTF-8", 
		"Body=method=auditNotes&systemNotes=Agent%20{agentName_url}%20ID%3A{uname}%20began%20offline%20mode%20{Checkstart_url}&agentNotes=", 
		LAST);

	


	lr_end_transaction("CDA_JIA_WOrder_04_UnCheckOfline",LR_AUTO);

	lr_think_time(atoi(lr_eval_string("{Thinktime}")));
	

	lr_start_transaction("CDA_JIA_WOrder_05_Logoff");

	web_url("stopPush", 
		"URL={url}/JacadaWorkspaceCommon/push/stopPush?extend=function(object)%20%7B%0D%0A%20%20%20%20return%20Object.extend.apply(this%2C%20%5Bthis%2C%20object%5D)%3B%0D%0A%20%20%7D", 
		"TargetFrame=", 
		"Resource=0", 
		"Referer={url}/CDA/Controller.jpf", 
		"Mode=HTML", 
		LAST);

	

	web_add_auto_header("Accept", 
		"image/jpeg, application/x-ms-application, image/gif, application/xaml+xml, image/pjpeg, application/x-ms-xbap, application/vnd.ms-excel, application/vnd.ms-powerpoint, application/msword, */*");

	web_url("Controller.jpf_3", 
		"URL={url}/CDA/Controller.jpf?logout=true&shouldRegisterSession=false&reason=9999&department=", 
		"TargetFrame=", 
		"Resource=0", 
		"RecContentType=text/html", 
		"Referer=", 
		"Snapshot=t35.inf", 
		"Mode=HTML", 
		EXTRARES, 
		"Url=USER/resources/themes/Jacada/images/WinFuse-disabled.gif", ENDITEM, 
		LAST);

	lr_end_transaction("CDA_JIA_WOrder_05_Logoff",LR_AUTO);

	

	return 0;
}
OutPut:
vuser_end.c(72): Notify: Parameter Substitution: parameter "Checkstart" =  "08/02/2013 10:36:43"
vuser_end.c(73): Notify: Saving Parameter "Checkstart_url = 08%2F02%2F2013%2010%3A36%3A43".

