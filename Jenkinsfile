pipeline {
		agent any

		environment {
			FRONTEND_IMAGE= "mern-frontend:jenkins"
			BACKEND_IMAGE= "mern-backend:jenkins"
			PORT= "5000"
			MONGO_URI= "mongodb://mongo:27017/taskdb"
		} 

		stages {
			stage('Checkout Code') {
				steps {
					git url: 'https://github.com/khalidnbg/devops-cours', branch: 'main'
				}
			}

			stage('Prepare .env') {
				steps {
					sh '''
						mkdir -p server
						cat > server/.env <<EOF
						PORT=$PORT
						MONGO_URI=$MONGO_URI
						EOF
					'''
				}
			}

			stage('Build Docker Images') {
				steps {
					sh '''
						echo "Building backend Image..."
						docker build -t $BACKEND_IMAGE ./server
						echo "Building frontend Image..."
						docker build -t $FRONTEND_IMAGE ./client --build-arg VITE_API_URL=http://localhost:5000/api
					'''
				}
			}

			stage('Run Docker Containers with Docker Compose') {
				steps {
					sh '''
						echo "Starting services with Docker Compose..."
						docker compose up -d

						echo "Showing running containers..."
						docker ps
					'''
				}
			}
		}
}