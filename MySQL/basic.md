# MySQL

Structured Query Language: 구조화된 CRUD(쿼리한다)

schme: == 데이터베이스

- 데이터베이스에 여러 권한을 부여할 수 있음, 읽기만 가능하거나 수정 가능 부분 권한 부여 가능

## DB 생성

CREATE DATABASE `데이터베이스명`

## DB 삭제

DROP DATABASE `데이터베이스명`;

## DB 열람

SHOW DATABASES;

## DB 선택

USE `데이터베이스명`

명령어는 대문자든 소문자든 상관없음(데이터베이스와 테이블 이름은 대소문자를 구별함)

## 테이블 생성

CREATE TABLE `student` (

`id`  tinyint NOT NULL ,

`name`  char(4) NOT NULL ,

`sex`  enum('남자','여자') NOT NULL ,

`address`  varchar(50) NOT NULL ,

`birthday`  datetime NOT NULL ,

PRIMARY KEY (`id`)

);

## 데이터 타입

CHAR 고정 문자 타입

VARCHAR 가변 문자 타입

TINYINT INT보다 작은 범위(-128 ~ 127)

INT 정수형

ENUM 정해져 있는 타입 강제

## 테이블 리스트

SHOW tables;

## 테이블 스키마 열람

DESC `테이블명`

## 테이블 제거

DROP TABLE `테이블명`

## 데이터 삽입

INSERT INTO `student` VALUES ('2', 'str', 'man', 'seoul', NOW());

## 데이터 변경

UPDATE `student` SET name='joo' WHERE id=1;

## 데이터 삭제

DELETE FROM student WHERE id = 2;

TRUNCATE student; (테이블의 전체 데이터 삭제)

## 데이터 조회

SELECT 칼럼명1, 칼럼명2

[FROM 테이블명 ]

[GROUP BY 칼럼명]

[ORDER BY 칼럼명 [ASC | DESC]]

[LIMIT offset, 조회 할 행의 수]

primary key

중복 X, 가장 고속, 한 개만 선언 가능

unique key

중복 X, 고속, 여러 개 선언 가능

normal

중복 O, 느림, 여러 개 선언 가능

foreign : 다른 테이블과의 관계성을 부여하는 키
