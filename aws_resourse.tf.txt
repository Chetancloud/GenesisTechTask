{\rtf1\ansi\ansicpg1252\cocoartf2636
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fswiss\fcharset0 Helvetica;}
{\colortbl;\red255\green255\blue255;}
{\*\expandedcolortbl;;}
\paperw11900\paperh16840\margl1440\margr1440\vieww11520\viewh8400\viewkind0
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural\partightenfactor0

\f0\fs24 \cf0 provider "aws" \{\
  region = var.region\
\}\
\
# Create VPC\
module "vpc" \{\
  source = "terraform/vpc/aws"\
  version = "3.11.0"\
  name = var.vpc_name\
  cidr = var.vpc_cidr\
  azs = var.vpc_azs\
  private_subnets = var.private_subnets\
  public_subnets = var.public_subnets\
  enable_nat_gateway = true\
  single_nat_gateway = true\
  create_database_subnet_group = true\
  create_database_security_group = true\
\}\
\
# Create EC2 Instance\
module "ec2" \{\
  source = "terraform/ec2-instance/aws"\
  version = "3.0.0"\
  name = var.ec2_name\
  ami = var.ec2_ami\
  instance_type = var.ec2_instance_type\
  vpc_security_group_ids = [module.ec2_security_group.this_security_group_id]\
  subnet_id = module.vpc.public_subnets[0]\
  key_name = var.ec2_key_name\
  associate_public_ip_address = true\
  root_block_device = \{\
    volume_size = var.ec2_root_volume_size\
    volume_type = "gp3"\
  \}\
  ebs_block_device = [\
    \{\
      device_name = var.ec2_data_device_name\
      volume_size = var.ec2_data_volume_size\
      volume_type = "gp3"\
    \}\
  ]\
  tags = \{\
    Name = var.ec2_name\
  \}\
\}\
\
# Create Load Balancer and Target Group\
module "alb" \{\
  source = "terraform/alb/aws"\
  version = "5.5.0"\
  name = var.alb_name\
  load_balancer_type = var.alb_type\
  security_groups = [module.alb_security_group.this_security_group_id]\
  subnets = module.vpc.public_subnets\
  enable_http2 = true\
  idle_timeout = var.alb_idle_timeout\
  tags = \{\
    Name = var.alb_name\
  \}\
  target_groups = [\
    \{\
      name_prefix = var.alb_target_group_name_prefix\
      backend_protocol = var.alb_target_group_backend_protocol\
      backend_port = var.alb_target_group_backend_port\
      target_type = var.alb_target_group_target_type\
      health_check = \{\
        path = var.alb_target_group_health_check_path\
      \}\
    \}\
  ]\
\}\
\
# Create RDS Instance\
module "rds" \{\
  source = "terraform/rds/aws"\
  version = "3.3.0"\
  identifier = var.rds_identifier\
  engine = var.rds_engine\
  engine_version = var.rds_engine_version\
  instance_class = var.rds_instance_class\
  allocated_storage = var.rds_allocated_storage\
  name = var.rds_name\
  vpc_security_group_ids = [module.rds_security_group.this_security_group_id]\
  subnet_ids = module.vpc.database_subnets\
  multi_az = var.rds_multi_az\
  db_name = var.rds_db_name\
  username = var.rds_username\
  password = var.rds_password\
  tags = \{\
    Name = var.rds_name\
  \}\
\}\
\
# Create Route53 records\
resource "aws_route53_record" "ec2_private_ip" \{\
  zone_id = var.route53_zone_id\
  name = var.ec2_record_name\
  type = "A"\
  ttl = "300"\
  records = [module.ec2.private_ip]\
\}\
\
resource "aws_route53_record" "al\
}