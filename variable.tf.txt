{\rtf1\ansi\ansicpg1252\cocoartf2636
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fswiss\fcharset0 Helvetica;}
{\colortbl;\red255\green255\blue255;}
{\*\expandedcolortbl;;}
\paperw11900\paperh16840\margl1440\margr1440\vieww11520\viewh8400\viewkind0
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural\partightenfactor0

\f0\fs24 \cf0 region = "us-west-2"\
\
# VPC variables\
vpc_name = \'93aws-vpc"\
vpc_cidr = "10.0.0.0/16"\
vpc_azs = ["us-west-2a", "us-west-2b", "us-west-2c"]\
private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]\
public_subnets = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]\
\
# EC2 instance variables\
ec2_name = \'93aws-ec2-instance"\
ec2_ami = "ami-123456789\'94\
ec2_instance_type = "t3.medium"\
ec2_key_name = \'93aws-key-pair"\
ec2_root_volume_size = 50\
ec2_data_device_name = "/dev/sdb"\
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural\partightenfactor0
\cf0 ec2_RDS_Port = 1433\
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural\partightenfactor0
\cf0 ec2_data_volume_size = 50\
\
# ALB variables\
alb_name = \'93aws-alb"\
alb_type = "application"\
alb_idle_timeout = 60\
alb_target_group_name_prefix = "my-target-group"\
alb_target_group_backend_protocol = "HTTP"\
alb_target_group_backend_port = 443\
alb_target_group_target_type = "instance"\
alb_target_group_health_check_path = "/"\
\
# RDS variables\
rds_identifier = \'93aws-rds-instance"\
rds_engine = "sqlserver-ex"\
rds_engine_version = "15.00.4073.23.v1"\
rds_instance_class = "db.t3.medium"\
rds_allocated_storage = 70\
rds_name = \'93aws-db\'94\
}