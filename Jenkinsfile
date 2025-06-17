pipeline {
    agent any // Chạy trên bất kỳ agent (máy) nào có sẵn

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning source code...'
                // Lấy code từ nhánh main của repo
                git branch: 'main', url: 'https://github.com/MrOwl715/.net-project.git'
            }
        }

        stage('Restore Packages') {
            steps {
                echo 'Restoring .NET packages...'
                // Chạy lệnh restore trên Windows agent
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Build project với cấu hình Release
                bat 'dotnet build --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Chạy unit test
                bat 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish Artifacts') {
            steps {
                echo 'Publishing build artifacts...'
                // Xuất bản kết quả build ra thư mục 'publish'
                bat 'dotnet publish --configuration Release --no-build -o ./publish'
            }
        }

        stage('Copy to Web Server') {
            steps {
                echo 'Copying files to wwwroot...'
                // Copy từ thư mục publish trong workspace vào thư mục của IIS
                // Lưu ý: Cần chắc chắn đường dẫn "c:\\wwwroot\\myproject" đã tồn tại
                bat 'xcopy "%WORKSPACE%\\publish" "c:\\wwwroot\\myproject" /E /Y /I /R'
            }
        }

        stage('Deploy to IIS') {
            steps {
                powershell '''
                Import-Module WebAdministration
                $siteName = "MySite"
                $physicalPath = "c:\\wwwroot\\myproject"  // Đã sửa lại đường dẫn này cho khớp với stage trên
                
                # Kiểm tra website đã tồn tại chưa
                if (-not (Test-Path "IIS:\\Sites\\$siteName")) {
                    # Tạo website mới nếu chưa có
                    New-Website -Name $siteName -Port 81 -PhysicalPath $physicalPath
                    Write-Output "Website '$siteName' created."
                } else {
                    Write-Output "Website '$siteName' already exists."
                }
                '''
            }
        }
    } // end stages
} // end pipeline