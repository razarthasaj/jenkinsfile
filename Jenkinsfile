import hudson.FilePath
import hudson.*

//--job VARs ---
dir_path="bud-qa/API/bud-api"
priority_build="5"
lable_name="CTF_GUI"
special_character='/$'

//--url VARs ---
url="https://staging.thisisbud.com"

//--setup all the jobs ---:
def job_list= [ "bud_api_accunts_test","bud_api_auth_test","bud_api_categories_test","bud_api_discovery_test","bud_api_insights_test","bud_api_lists_test","bud_api_me_test","bud_api_products_test","bud_api_transactions_test" ]

//--create folder ---
folder("API")

//--setup all email jobs ---
for (int i=0; i < job_list.size(); i++) {
        setup_all_jobs = ("${job_list.get(i)}")

  		def Job = freeStyleJob("API/${setup_all_jobs}")
  				
        Job.with {  
			//--job description --- 	
            description "Job DSL has setup this job named $setup_all_jobs"
           //--label "Set up all jobs" ---
           logRotator {
                      daysToKeep(1)
                      numToKeep(5)
           }

          //--allow concurrent builds ---
          //concurrentBuild(true)
          
          //--restrict where it can run this build ---
          label("${lable_name}") 

           //--clean workspace before build ---  
            wrappers {
            		  preBuildCleanup()

            		  } 
    
       	  //--setup priority build ---
          configure { project ->
            project / 'properties' / 'hudson.model.ParametersDefinitionProperty' / 'parameterDefinitions' << 'hudson.model.StringParameterDefinition'{
              name("BuildPrority")
              defaultValue("${priority_build}")
            }
          }//--end StringParameterDefinition !!!
          
		//--copy file to node --- 
      	configure { project ->
            project / 'buildWrappers' << 'com.michelin.cio.hudson.plugins.copytoslave.CopyToSlaveBuildWrapper'{
					includes("${dir_path}${special_character}{JOB_BASE_NAME}.robot,bud-qa/API/Utils/**")
              		excludes()
              		flatten(false)
              		includeAntExcludes(false)
              		hudsonHomeRelative(false)
              		relativeTo("userContent")
            	}
          }//--end CopyToSlave !!!
		  
		  //--setup batch file with pybot ---
          steps {
            	batchFile( "cd ${dir_path}/\npybot -v url:${url} --timestampoutputs %JOB_BASE_NAME%.robot" )
          		} 
         
          //--archive artifacts for RF ---
          publishers {
            
            publishRobotFrameworkReports {
              		passThreshold(100.0)
              		unstableThreshold(100.0)
              		onlyCritical()
              		outputPath("${dir_path}/") 
              		outputFileName("**/*output*.xml") 
              		reportFileName("**/*report*.html") 
              		logFileName("**/*log*.html") 
              		otherFiles("*.jpg,*.png") 
       		 } 
            
            //--setup Email to recipent ---
            mailer('razart@thisisbud.com', false, false)
            //--cleanup everything and don't fail the build if cleanup fails 
                  wsCleanup {
                             cleanWhenSuccess(true)
                             cleanWhenUnstable(true)
                             cleanWhenFailure(true) 
                             cleanWhenNotBuilt(true)
                             cleanWhenAborted(true) 		
            	}//--end wssCleanup !!! 
          }//--end publishers !!!          
   	}//--end Job !!!
	
} //--end of for loop !!!
