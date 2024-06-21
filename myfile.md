Current Drives:

Google:

CloudGv00 (Myb6tbc)

Family

Kids

MamaBetty

Sue

MEYE (Myb6tbc)

notMEGA (Myb6tbc)

MEGA

Videos (Myb12tb)

DepthQ\_Demo-B\_1280x1440\_VC-1

Kathy

KathyDVD

KathyVHS

Video1948to1969

Video1985to1999

Video1985

Video1988

Video1989

Video1990

Video1991

Video1992

Video1993

Video1994

Video1995

Video1996

Video1997

Video1998

Video1999

Video2000to2009

Video2000

Video2001

Video2002

Video2003

Video2004

Video2005

Video2006

Video2007

Video2008

Video2009

Videos2010to2013

Video2010

Video2011

Video2012

Video2013

Videos2014to2016

Video2014

Video2015

Video2016

Video2017to2019

Video2017

Video2018

Video2019

Video2020to2021

Video2020

Video2021

Video2022toPresent

Video2022

Video2023

Pictures (Myb6tbc)

AllPictures

Artin

Bianka

Mae

OldPercyPictures

Pic19xxto1999

Pic2000to2009

Pic2010to2013

Pic2014to2016

Pic2017to2019

Pic2020topresent

Share (Myb6tbc)

Family

Kids

MamaBetty

Sue

Docs (Myb6tbc)

MyMusic

SierraClub

Videodoc

MyDocumentsMerge

GTKrs

Iced

Church

DadsFiles

Johnphone

rfoBasic

Backup(easystore-8tba)

JohnComputer

JohnComputer20190904

JohnComputer20200508

JohnComputer20200715

JohnComputer20200901

JohnComputer20201107

ElyComputer

Ely20100131Merge

ElyCompBkup20190101

ElyComputer20181006

ElyComputer20200508

ElyComputer20200712

ElyComputer20200901

ElyComputer20201107

ElyFiles20190531

ElyPhone20160604Merge

OpticalDiskBackups

General

Backup20210327

Backup20210625

Backup20210811

\...

Phone

Phone20220707

ClonezillaBackups

QUARTERLY BACKUP

run Dir /s \> filelistyyyymmdd.txt

backup computers using freefilesync (FFS)

compare file size

export filelist.csv before update

update

Ely onto 3tb drive

John onto 6tba drive

copy Ely files since last bluray backup to Acer with FFS

copy John files since last bluray backup to Acer with FFS.

duplicate delete John files.

duplicate delete Ely with John

backup to bluray

copy backup to cloud

UPDATE BACKUP DATABASE:

OPTION A USING CDTREE:

take Cdtree EXPORTED XML file

STEP 1

convert the Cdtree xml file to UTF-8

Execute:

iconv -f utf-16le -t UTF-8 CdTree20231011all.xml -o convt20231011all.xml

STEP 2

using RUST-ICED PROGRAM cdtxml2csvbar,

create input file to create database.

use the above file convt20231011all.xml for xml input will create output
file of

convt20231011all.xml\_\_tmpcvs

with error message file of

convt20231011all.xml\_\_tmperr

STEP 3

create the database

Execute:

sqlite3 bkinit.db3 \< importconvt20231011all.sql 2\> bkinitout.txt

where importconvt20231011all.sql is

CREATE TABLE blubackup (

refname TEXT NOT NULL,

filename TEXT NOT NULL,

dirname TEXT NOT NULL,

filesize INTEGER NOT NULL,

filedate TEXT NOT NULL,

md5sum TEXT,

locations TEXT ,

notes TEXT ,

PRIMARY KEY (refname, filename, dirname, filesize));

.separator \|

.import convt20231011all.xml\_\_tmpcvs blubackup

use THESE set of steps to add an new backup.

STEP 5

since the Cdtree catagories are set by year, just export the current
year.

CdTree20231209bk2023.xml

STEP 6

convert the Cdtree xml file to UTF-8

Execute:

iconv -f utf-16le -t UTF-8 CdTree20231209bk2023.xml -o convtjust2023.xml

STEP 7

using RUST-ICED PROGRAM cdtxml2csvbar,

create input file to create TEMPORARY database.

use the above file convtjust2023.xml for xml input will create output
file of

convtjust2023.xml\_\_tmpcvs

with error message file of

convtjust2023.xml\_\_tmperr

STEP 8

edit convtjust2023.xml\_\_tmpcvs and remove all other backups except
current.

i.e. only keep those with refname of bk20231209-

STEP 9

create the temporary database

Execute:

sqlite3 bk1209.db3 \< importconvtjust2023.sql 2\> bk1209out.txt

where importconvtjust2023.sql is

CREATE TABLE blubackup (

refname TEXT NOT NULL,

filename TEXT NOT NULL,

dirname TEXT NOT NULL,

filesize INTEGER NOT NULL,

filedate TEXT NOT NULL,

md5sum TEXT,

locations TEXT ,

notes TEXT ,

PRIMARY KEY (refname, filename, dirname, filesize));

.separator \|

.import convtjust2023.xml\_\_tmpcvs blubackup

STEP 10

DUMP THE Temporary db

Execute:

sqlite3 bk1209.db3 .dump \> dumpbk1209.sql

STEP 11

make a copy of the current backup database i.e. bkinit.db3

Execute:

cp -p bkinit.db3 bkinit01.db3

STEP 12

insert files into main db

Execute:

sqlite3 bkinit.db3 \< dumpbk1209.sql 2\> bk1209insertout.txt

ignore error of: table blubackup already exists.

Size of original bkinit01 plus bk1209 should equal size of bkinit.

use these steps to update backup disc database with md5sum.

STEP 13

start running RUST-ICED PROGRAM bkmd5sum

load the bluray disc

confirm that internal matches handwritten label and it is in the backup
disc database

the output list is used to update the backup disc database

STEP 14

make a copy of CURRENT DATABASE bkinit.db3

Execute:

cp -p bkinit.db3 bkinit02.db3

STEP 15

start running RUST-ICED PROGRAM bklistdbupd01

specify the database and the listing from PROGRAM bkmd5sum

specify an output directory

try test run and review output

this is a slow process so start progress button will keep you up to date
on run

most of the time the dates do not match.

Try hour, but some go on to the next day so may have to include date

ways of excluding items is to edit the list or add an exclude file
(directories only).

when happy with test output then run update

OPTION B USING JUST BACKUP LIST:

STEP 1

start running RUST-ICED PROGRAM bkmd5sum

load the bluray disc

confirm that internal matches handwritten label and it is in the backup
disc database

the output list is used to update the backup disc database:
bkyyyymmddnn.list

STEP 2

create a temporary database using the output list (you can concat them
to reduce the number of update runs): bkyyyymmddnn.list

Execute:

sqlite3 bkyyyymmddnn.db3 \< importbkyyyymmddnn.sql 2\> bkyyyymmddnn.txt

where importbkyyyymmddnn.sql is

CREATE TABLE blubackup (

filename TEXT NOT NULL,

filesize INTEGER NOT NULL,

filedate TEXT NOT NULL,

dirname TEXT NOT NULL,

refname TEXT NOT NULL,

md5sum TEXT,

locations TEXT ,

notes TEXT ,

PRIMARY KEY (refname, filename, dirname, filesize));

.separator \|

.import bkyyyymmddnn.list blubackup

STEP 3

DUMP THE Temporary database(s)

Execute:

sqlite3 bkyyyymmddnn.db3 .dump \> bkyyyymmddnn.sql

STEP 4

edit each bkyyyymmddnn.sql do a find

blubackup

and replace all with

blubackup(filename, filesize, filedate, dirname, refname, md5sum,
locations, notes)

Since the table is already created, you can delete the create table
section at the top.

the original database table was defined in a different order.

STEP 5

make a copy of the current backup database i.e. bkinit.db3

Execute:

cp -p bkinit.db3 bkinit01.db3

STEP 6

insert files into main db

Execute for all temporary databases created:

sqlite3 bkinit.db3 \< bkyyyymmddnn.sql 2\> bkyyyymmddnnsql.txt

ignore error of: table blubackup already exists.

Size of original bkinit01 plus bkyyyymmddnn should equal size of bkinit.

No additional steps since the listings already have the md5sum value.

HARDDRIVE DATABASE EVALUATION and MAINTAINING

using hdmd5sum create md5sum listing for all disks.

Backup files to harddrive and cloud.

STEP A

Start with largest file and create database.

Execute:

sqlite3 hdinit.db3 \< importhd6tbaJohnComputer.sql 2\> hdinitout.txt

where importhd6tbaJohnComputer.sql is

CREATE TABLE harddrive (

filename TEXT NOT NULL,

filesize INTEGER NOT NULL,

filedate TEXT NOT NULL,

dirname TEXT NOT NULL,

refname TEXT NOT NULL,

md5sum TEXT ,

locations TEXT ,

notes TEXT ,

PRIMARY KEY (refname, filename, dirname, filesize));

.separator \|

.import hd6tbaJohnComputer.list harddrive

STEP B

next file and create temporary file

Execute:

sqlite3 hdwindowsC.db3 \< importhdwindowsC.sql 2\> hdwindowsCout.txt

where importhdwindowsC.sql is

CREATE TABLE harddrive (

filename TEXT NOT NULL,

filesize INTEGER NOT NULL,

filedate TEXT NOT NULL,

dirname TEXT NOT NULL,

refname TEXT NOT NULL,

md5sum TEXT ,

locations TEXT ,

notes TEXT ,

PRIMARY KEY (refname, filename, dirname, filesize));

.separator \|

.import hhdwindowsC .list harddrive

STEP C

DUMP THE Temporary db

Execute:

sqlite3 hdwindowsC.db3 .dump \> dumphdwindowsC.sql

STEP D

make a copy of hdinit.db3

Execute:

cp -p hdinit.db3 hdinit01.db3

STEP E

insert files into main db

Execute:

sqlite3 hdinit.db3 \< dumphdwindowsC.sql 2\> hdwindowsCdumpout.txt

ignore error of: table harddrive already exists.

Size of original hdinit01 plus hdwindowsC should equal size of hdinit.

REPEAT STEPS B THROUGH E FOR ALL LISTS.

OPTION OLD WITH CDTREE:

cdtree bluray discs

from cdtree export to xml

convert xml to character system to utf8 because of rust program
compatibility

file -i cd\....xml to get character system which should be utf-16le

need to convert to utf-8 for rust read it.

iconv -f utf-16le -t UTF-8 \< cd\....xml \> cd\...utf8.xml

gtk rust program BkupEval03 to convert xml to file. output is
cd\....cdlist

sort file

sort cd\....cdlist -k 1 \> cd\....cdsort

get fresh list of all drives:

gtk rust program BkupEval03 to list harddrive. output is hd\....hdlist

sort file

sort hd\....hdlist -k 1 \> hd\....hdsort

COMPARING CLOUD WITH HARDDRIVE

Need to run the following terminal commands before using iced program
MegaParse

rclone lsf \--files-only -R \--csv \--format pst /pathofHDdirectory \|
sort \> outputfile1

but have changed it to not include date (t), because time is off by an
hour or minute.

rclone lsf \--files-only -R \--csv \--format ps /pathofHDdirectory \|
sort \> outputfile1

(rclone lsf \--files-only -R \--csv \--format ps
/media/jp/Myb12tb/MEGA/Video \| sort \> hdVideo)

[]{#anchor}mega-ls -l -r \--time-format=ISO6081\_WITH\_TIME
/cloudpathofdirectory \> outputfile2

(mega-ls -l -r \--time-format=ISO6081\_WITH\_TIME /Video \> megaVideo)

iced Run MegaParse

Run this app using mega outputfile2 as input file

and the second rclone (without date).

It will bring up Meld.

Creates two files: one with datetime and the other without datetime

Need to run the following terminal command after using MegaParse

sort targetfile \> outputfile3

compare outputfile1 with outputfile3. I use Meld.

First compare with out time then try with time.

In MEGAcmd source, megacmdexecuter.cpp line 1347 & 1447 change 10 to 11,

mega-login jpercyasnet\@yahoo.com password

mega-cd Video/Video2022toPresent/Video2023/202310Egypt/pictures

mega-rm -r pic20231017

STEP F2

need a program to go through the two databases. Ideally the smaller
database as input and search through the other database for filename.
Check size, date and md5sum. If md5sum not present output as separate
file. And continue. Each will allow 10 locations. If larger, output as
other separate file. Location is the reference name plus smd. S for same
size. M for same md5sum and d for same date. At least one must exist. If
found make sure location does not already exist. Separate locations by
comma (,) and smd and location by dash (-)

TO SEE IF HD & MEGA MATCH

with time:

rclone lsf \--files-only -R \--csv \--format pst /pathofHDdirectory \|
sort \> outputfile1

without time:

rclone lsf \--files-only -R \--csv \--format ps /pathofHDdirectory \|
sort \> outputfile1

mega-login email password

this generates list of files takes awhile.

if mega-logout, will remove list of files and login will be regenerated.

mega-ls -l -r \--time-format=ISO6081\_WITH\_TIME /cloudpathofdirectory
\> outputfile2\"

Run MegaParse using outputfile2 as input file and targetfile is output

sort targetfile \> outputfile3

compare outputfile1 with outputfile3. I use Meld (not that good, try old
way)

diff outputfile1 outputfile3 \> compare.list

make program like megaremove to take those files with "\<" and prefix
them with:

"mega-rm -r "

DUPLICATE FILES REMOVAL

Use fclones dry run output.

fclones group ./testloc \> dups.txt

fclones remove \--keep-path \'\*\*/rawvideo/\*\*\' -o dryex.txt \<
dups.txt \--dry-run 2\>dryrun01.txt

Generate mega move from output.

format: mega-mv frompath/name topath

no output is generated

Run mega move.

If ok, Run fclones move

fclones move \--keep-path \'\*\*/rawvideo/\*\*\' -o dryex.txt topath \<
dups.txt 2\>dryrun01.txt

See if HD & MEGA match.

Delete moved files on both HD & MEGA manually.
