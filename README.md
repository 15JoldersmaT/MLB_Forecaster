# MLB_Predictor
Attempt to predict MLB outcomes using a random forest model.


#How it was made
Create a "gorgon" csv file through gathering data from :
https://www.sports-reference.com/
And some simple sql, all done in Microsoft SQL Server Management Studio


#Syncing games (getting opponent data for prediction)
SELECT a.W_L, a.Team,a.Gm, a.RYTD, a.RAYTD, a.RankR, a.GBR, a.D_1_N_0, a.Home, a.RealStreakN, a.RealStreakW, b.Team Teamb, a.Gm Gmb, b.RYTD RYTDb, b.RAYTD RAYTDb, b.RankR RankRb, b.GBR GBRb, b.RealStreakN RealStreakNb, b.RealStreakW RealStreakWb
FROM dbo.MLB a
INNER JOIN dbo.MLB b ON a.Opp = b.Tm and a.Date = b.Date ;

#Table creation
USE [Baseball]
GO

/****** Object:  Table [dbo].[MLB]    Script Date: 9/11/2023 10:48:31 PM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[MLB](
	[Team] [nvarchar](50) NOT NULL,
	[Year] [smallint] NOT NULL,
	[Gm] [tinyint] NOT NULL,
	[Date] [nvarchar](50) NOT NULL,
	[Tm] [nvarchar](50) NOT NULL,
	[Home] [bit] NOT NULL,
	[Opp] [nvarchar](50) NOT NULL,
	[W_L] [nvarchar](50) NOT NULL,
	[R] [tinyint] NOT NULL,
	[RYTD] [smallint] NOT NULL,
	[RA] [tinyint] NOT NULL,
	[RAYTD] [smallint] NOT NULL,
	[Inn] [tinyint] NULL,
	[Record] [nvarchar](50) NOT NULL,
	[Rank] [tinyint] NOT NULL,
	[RankR] [tinyint] NOT NULL,
	[GB] [float] NOT NULL,
	[UP] [smallint] NOT NULL,
	[GBR] [float] NOT NULL,
	[Win] [nvarchar](50) NOT NULL,
	[Loss] [nvarchar](50) NOT NULL,
	[Save] [nvarchar](50) NULL,
	[Time] [time](7) NOT NULL,
	[D_1_N_0] [bit] NOT NULL,
	[Attendance] [int] NULL,
	[cLI] [float] NULL,
	[Streak] [nvarchar](50) NOT NULL,
	[StreakN] [tinyint] NOT NULL,
	[StreakW] [smallint] NOT NULL,
	[RealStreakN] [tinyint] NOT NULL,
	[RealStreakW] [smallint] NOT NULL,
 CONSTRAINT [PK_MLB] PRIMARY KEY CLUSTERED 
(
	[Team] ASC,
	[Year] ASC,
	[Gm] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO


#Select from table to create "gorgon" csv file
SELECT a.W_L, a.Team,a.Gm, a.RYTD, a.RAYTD, a.RankR, a.GBR, a.D_1_N_0, a.Home, a.RealStreakN, a.RealStreakW, b.Team Teamb, a.Gm Gmb, b.RYTD RYTDb, b.RAYTD RAYTDb, b.RankR RankRb, b.GBR GBRb, b.RealStreakN RealStreakNb, b.RealStreakW RealStreakWb
FROM dbo.MLB a
INNER JOIN dbo.MLB b ON a.Opp = b.Tm and a.Date = b.Date ;

You basically use the same stats when running the models to make a prediction

There are two predictions made, one from the perspective of the first team, and the reverse
Ideally there would be a mismatch
W
L

Or 
L
W

Not 
W
W

Or 

L
L
