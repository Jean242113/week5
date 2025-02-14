1.
create database `website`;

2.
create table website.member(
    id bigint primary key auto_increment comment 'Unique ID',
    name varchar(255) not null comment 'Name',
    username varchar(255) not null comment 'Username',
    password varchar(255) not null comment 'Password',
    follower_count int unsigned not null default 0 comment 'Follower Count',
    time datetime not null default current_timestamp comment 'Signup Time'
);

3-1.
insert into member (name, username, password) values ('test', 'test', 'test');
insert into member (name, username, password, follower_count) values ('alice', 'alice123', 'pass1', 10);
insert into member (name, username, password, follower_count) values ('bob', 'bob456', 'pass2', 25);
insert into member (name, username, password, follower_count) values ('charlie', 'charlie789', 'pass3', 5);
insert into member (name, username, password, follower_count) values ('david', 'david999', 'pass4', 50);



3-2.
select * from member;

3-3.
select * from member order by time desc;

3-4.
select * from member order by time desc limit 3 offset 1;

3-5.
select * from member where username = 'test';

3-6.
select * from member where name like '%es%';

3-7.
select * from member where username = 'test' and password = 'test';

3-8.
update member set name = 'test2' where username = 'test';

4-1.
select count(*) from member;

4-2.
select sum(follower_count) from member;

4-3.
select avg(follower_count) from member;

4-4.
select avg(follower_count) 
from (select follower_count from member order by follower_count desc limit 2) as top2;

5-1.
create table website.message(
    id bigint primary key auto_increment comment 'Unique ID',
    member_id bigint not null comment 'Member ID for Message Sender' ,
    content varchar(255) not null comment 'Content',
    like_count int unsigned not null default 0 comment 'Like Count',
    time datetime not null default current_timestamp comment 'Publish Time',
    foreign key (member_id) references website.member(id)
);

5-2.
select m.id, m.content, m.like_count, m.time, mem.name as sender_name
from message m
join member mem on m.member_id = mem.id order by id;



5-3.
select m.id, m.content, m.like_count, m.time, mem.name as sender_name
from message m
join member mem on m.member_id = mem.id
where mem.username = 'test';

5-4.
select avg(m.like_count) as avg_like_count
from message m
join member mem on m.member_id = mem.id
where mem.username = 'test';

5-5.
select mem.username, avg(m.like_count) as avg_like_count
from message m
join member mem on m.member_id = mem.id
group by mem.username;
