pipeline {
agent any
stages {
        stage('Build') {
            steps {
                echo 'Building ..'
				sh '''
						set +x
						echo -e "\n\n**************************** This is a Build JOB **************************** "
           					 cd /var/lib/jenkins/workspace/sample_pipeline1
						  
						  echo -e "\n\nSTEP 1:	Initiating Build (using make)........"
								make -f mkfile2
								make -f mkfile2 clean

						if [[ -x "XYZ.exe" ]]; then  
							echo -e "\nSTEP 4:	Build Successful"
							echo "       	BUILD_FILE: XYZ.exe"	
						else
							echo -e "**************** ERROR *********************\n"
							echo "Build failure - No executable file generated"
							echo -e "********************************************\n"
							exit 1
						fi    

						echo -e "\n************************************************************************** "
				'''
				
            }
        }
        stage('deploy') {
            steps {
                echo 'Deploying to TEST environment..'
				sh '''
					set +x
          				pwd=/var/lib/jenkins/workspace/sample_piepeline1
					file="$(pwd)/XYZ.exe"
					
										
				echo -e "\n\n**************************** This is a Deploy JOB $file **************************** "
				
				if [[ -f "$file" ]]; then 
				  echo -e "\nSTEP 1: Deploying $file with default input........"
				   $file -v -i <<<"5 4 5 6 012"
				  
                                   
				   if [ $? -eq 0 ]; then
				   echo -e "\nSTEP 2:Deployment Successful - Build pushed to artifactory"
				    
				    echo " ARTIFACTORY_PATH: /home/ec2-user/builds/ "
				   fi			     
				    
				    else  
					  echo -e "**************** ERROR *********************\n"
					  echo "Deployment failure - XYZ.exe cannot be executed "
					  echo -e "********************************************\n"
				       exit 1
				  fi
				    
				
				'''
            }
        }
        stage('test') {
            steps {
                echo 'Testing....'
				sh '''
				set +x
					pwd=/var/lib/jenkins/workspace/sample_piepeline1
					file="$(pwd)/XYZ.exe"
									
				echo -e "\n\n**************************** This is a TEST JOB for $file**************************** "
				if [[ -f "$file" ]]; then 
				count=1
				
				for i in {1..5}
				do
					oe=$((1000 + RANDOM % 10000))
					fact=$((1 + RANDOM % 15))
					num1=$((1 + RANDOM % 100))
					num2=$((1 + RANDOM % 1000))
					
						echo -e "\n-------------------------- TEST_CASE ($count) --------------------------"
						$file -v -i <<<"$fact $oe $num1 $num2 $oe"
					
					if [ $? -eq 0 ]; then
						echo -e "\n      RESULT -> TEST_CASE (${count}): SUCCESS" 
					else  
						echo -e "\n      RESULT -> TEST_CASE (${count}): FAILED"
						exit 1
					fi

					echo "-------------------------------------------------------------------"
					((count++))
				done
				else 
					 echo -e "**************** ERROR *********************\n"
					  echo "Build - xyz.exe notfound $file "
					  echo -e "********************************************\n"
					  exit 1
				fi
				'''
					}
				}
    }
}
