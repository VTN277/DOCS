# Tóm tắt: Cơ sở hạ tầng toàn cầu của AWS

## Quy mô của AWS
- 31 Regions
- 99 Availability Zones
- 450+ Points of Presence
- 400+ Edge Locations và 13 Regional Edge Caches
- Có mặt tại 245 quốc gia
- 32 Local Zones
- 115 Direct Connect Locations

## Các khái niệm chính

### Region
- Khu vực vật lý trên thế giới cung cấp dịch vụ điện toán đám mây AWS
- Mỗi Region độc lập và bao gồm nhiều Availability Zone (AZ)

#### Tiêu chí chọn Region:
1. Tuân thủ quy định (compliance)
2. Gần người dùng (giảm độ trễ)
3. Dịch vụ có sẵn
4. Giá cả

### Availability Zone (AZ)
- Trung tâm dữ liệu độc lập trong một Region
- Mỗi Region thường có ít nhất 3 AZ
- Giúp đảm bảo tính khả dụng cao (HA) cho ứng dụng

### Local Zone
- Cơ sở hạ tầng gần các trung tâm dân cư và công nghiệp lớn
- Cung cấp các dịch vụ AWS chọn lọc

### Edge Location
- Mạng lưới các điểm phân phối (Point of Presence) toàn cầu
- Hỗ trợ dịch vụ như Amazon CloudFront và Amazon Route 53
- Giảm độ trễ và tăng tốc độ truy cập cho người dùng cuối

## Tổng quan về các dịch vụ AWS chính

1. Computing & Container
   - Amazon EC2, AWS Lambda, Amazon EKS, Amazon ECS, AWS Fargate

2. Storage
   - Amazon S3, Amazon EBS, Amazon EFS, AWS Backup

3. Networking
   - Amazon VPC, Elastic Load Balancing, Amazon CloudFront, Amazon Route 53

4. Database
   - Amazon RDS, Amazon DynamoDB, Amazon Aurora, Amazon ElastiCache

5. Security & Identity
   - AWS IAM, Amazon Cognito, AWS WAF, AWS Shield

6. Management & Governance
   - AWS CloudFormation, AWS CloudTrail, Amazon CloudWatch, AWS Config

7. Messaging & Application Integration
   - Amazon SQS, Amazon SNS, Amazon EventBridge, Amazon API Gateway

8. Deployment & Automation
   - AWS CodePipeline, AWS CodeBuild, AWS CodeDeploy, AWS CloudShell

9. Migration
   - AWS Database Migration Service, AWS Application Migration Service

10. Big Data & Analytics
    - Amazon Redshift, Amazon QuickSight, AWS Glue, Amazon EMR

11. AI & Machine Learning
    - Amazon SageMaker, Amazon Rekognition, Amazon Polly, Amazon Translate

12. Các lĩnh vực khác
    - IoT, Media Services, Game, Quantum technologies, Robotics, Satellite, VR & AR, Blockchain

