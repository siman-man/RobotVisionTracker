# RobotVisionTracker
Robot Detection and Tracking of Objects in Industrial Setting

Prize Distribution
The best 5 performers in this contest according to system test results will receive the followings prizes:
上位5名には以下の賞金が与えられます

1. place: $10000
2. place: $5000
3. place: $2500
4. place: $1500
5. place: $1000

Background
背景

Firm A is building a next generation robotics platform that will change the game in field service operations 
A社は資産の検査と修復を含むゲームサービス業務を変える、次世代ロボティクスプラットフォームを行っており。
including asset inspection and repair. Firm A has defined a host of high value use cases and applications across 
効果値のユースケースと、フィールドエンジニアや他の産業労働者がより重要な、業界全体でのアプリケーションのホストを定義している。
industry that will support field engineers and other industrial workers be more productive and, more importantly, 
perform their jobs safely.

For one example high value use case, the company would like for a robot to detect and track a freight railcar 
例として、会社ではロボットがハンドルを把握することが出来るように、貨物鉄道車両のブレーキ開放ハンドル、オブジェクトと検出し、追跡を行えるように
brake release handle, the object of interest (OOI), so that the robot can grasp the handle.
したいと思っています。

Your task is to develop an algorithm that can detect and track the OOI in video frames. The OOI is typically 
あなたの目的は、ビデオフレームにおけるOOIを検知し追跡するアルゴリズムを開発することです。OOIは通常0.5インチの丸鋼棒からなるハンドルを形成するように
made of 0.5 inch round steel rod, bent to form a handle. Examples of the varieties of the OOI appear below. 
折り曲げられています。したにOOIの種類を示します。
The point marked blue is the point to be identified if present in a frame. More details follow.
青いマークのところは、フレーム内に存在する場合に識別することが出来ます。

Your algorithm will receive the following input:
あなたのアルゴリズムは次の入力を受け取ることができます。

Random samples of contiguous frames of videos shot in stereo ("Training Data"). Some frames will contain the OOI, 
ビデオの連続したフレームがランダムサンプルとして与えられます。いつくかのフレームにはOOIが含まれています。
others will not, and the samples will have 10 frames each.
他のものにはありません。サンプルは10フレームごとです。

The Training Data consists of videos containing frames, all shot in 640x480 resolution with the same camera.
トレーニングデータにはビデオフレームが含まれており、全て640 x 480の同じカメラで撮影されたものです。

Stereo camera calibration was performed with OpenCV. More information on camera calibration can be found here.
ステレオカメラのキャリブレーションは、OpenCVで行いました。カメラキャリブレーションの詳細については、ここで見ることが出来ます。
The left and right camera calibration parameters can be downloaded here and here.
左右のカメラキャリブレーションパラメータは、こことここからダウンロード出来ます。

If the OOI appears in the sample, then it will be marked in every frame as a point (x,y) according to the 
OOIがサンプル上に現れた時、それはある条件下でマークされています。
following convention, which defines the "ground truth" of the OOI's presence and location. As seen from the convention, 
the OOI is marked in 3 scenarios:

- When the OOI is in direct line of sight.

- When it is occluded by something in front of it.

- When the brake lever itself is occluding the point.

Please do look at the convention PDF in the link above.
上記リンクのPDFを確認して下さい。

The Training Data can be downloaded here.
トレーニングデータはこちらからダウンロードすることが出来ます


* 実装

あなたの目的はトレーニングおよび検証用の関数を作成することです。

int[] imageDataLeft and int[] imageDataRight contains the unsigned 24 bit image data. The data of each pixel 
imageDataLeftおよびImageDataRightは24bitの符号なし整数値が格納されております。 各データのピクセルは

is a single number calculated as 2^16 * Red + 2^8 * Green + Blue, where Red, Green and Blue are 8-bit RGB components 
2^16 * 赤色  + 2^8 * 緑色 + 青色が各8bitごとに格納されています。

of this pixel. The size of the image is 640 by 480 pixels. Let x be the column and y be the row of a pixel, then 
                画像のサイズは640 * 480となっております。            xが列、yが行となっております

the pixel value can be found at index [x+y*640] of the imageData array.
各ピクセルはimageData上の配列に格納されており[x+y*640]で取り出すことが出来ます。


Firstly, your training method will be called multiple times. You can use this method to train your algorithm on 
まず最初にあなたのトレーニング用のメソッドが何回も呼ばれます。              あなたはこの関数を使用することで、トレーニングデータを学習することが

the supplied training data. If your training method returns the value 1, then no more data will be passed to your 
出来るようになります。             もしあなたのメソッドが1の値を返す場合、               学習フェーズは終了し、検証フェーズに入ります。

algorithm, and the testing phase will begin. Data for each video frame of multiple videos will be sequentially passed 
                                             複数のビデオデータは、連続的にあなたの関数に渡されます。
to your method. All the video frames available for each training video will be passed to your algorithm. The number 
                 すべてのビデオフレームをあなたの学習アルゴリズムで使用することが出来ます
of frames for each video may differ. The ground-truth location for the OOI for each frame will also be provided in 
いくつかのフレームは異なっているかもしれません。    ground-truthの位置はleftX, leftY, rightX, rightYで与えられます。

leftX, leftY, rightX and rightY. A negative value indicates that the OOI was not detected in that frame.
                                  負値が示すのはOOIはフレームの中で検知出来ないかもしれません。                    

Once all training images have been supplied, doneTraining() will be called. This will signal that your 
全ての学習用画像の共有が終了したら、doneTraining()が呼び出されます。

solution should do any further processing based on the full set of training data.
ここでは、学習後にあなたが後処理を行いたい場合に、ここで処理を行う場所です。


Finally, your testing method will be called 50 times. The first 10 times it will contain contiguous frames 
最後に、あなたの検証用関数が50回呼び出されます。               最初の10回は連続的なフレームを持つビデオ情報です。

from a video. The next 10 times it will be contiguous from a second video. And so on... The array you return 
               次の10回は連続的なフレームを持つビデオ情報です。 それがどんどん続きます。
should contain exactly 4 values. Returning any point outside the bounds of the image will indicate that you 
あなたは4つ値をもつ配列を返す必要があります。 画像の境界の外側の任意の点を返すことは、画像内のOOIを検出できなかったことが示されます。  

did not detect the OOI in the image. Each element in your return should contain the following information:
                                      各要素には次の情報が含まれている必要があります。


- leftX - estimated x-coordinate for the point in the left image
leftX: 予測した左上のx座標

- leftY - estimated y-coordinate for the point in the left image
leftY: 予測した左上のy座標

- rightX - estimated x-coordinate for the point in the right image
rightX: 予測した右下のx座標

- rightY - estimated y-coordinate for the point in the right image
rightY: 予測した右下のy座標

The videos used for testing as well as the starting frame within the video will be selected randomly, 
検証で使用するビデオ映像はランダムにフレームが選択される。

so it is possible to have repetitions or intersection of frames during the testing phase.
そのため、検証フェーズはフレームの重なり等が発生する可能性があります。


検証とスコア

サンプル1、検証5、システムテスト10です

69のビデオは20, 10, 39に分割されており、最初の20ビデオはローカルテスト環境で使用できます。

Example tests: 15 (out of the set of 20) videos used for training, 5 for testing.

- Example: 15のトレーニングデータに、5つの検証データ

Provisional tests: 20 videos used for training, 10 for testing.

- 事前テスト: 20のトレーニングデータに、10の検証データ

System tests: 20 videos used for training, 39 for testing.

- システムテスト: 20のトレーニングデータに、39の検証データ

Your algorithm's performance will be quantified as follows.
あなたのアルゴリズムは次のように評価されます。


xr, yr: True x and y-coordinates for the OOI in the image (in units of pixels)
xr, yr: OOIの正しいx座標及び、y座標

xe, ye: Estimated x and y-coordinates for the OOI in the image (in units of pixels)
xe, ye: 予測されたx座標及び、y座標

dr = sqrt( (xe - xr)*(xe - xr) + (ye - yr)*(ye - yr) )

leftR[DIST] = percentage of left image frames whose dr <= DIST pixels

rightR[DIST] = percentage of right image frames whose dr <= DIST pixels

Note: In case of the OOI not being visible in the frame, the detection will be 
注意: OOIが検出されないフレームにおいて、あなたが正しく検出されていないことを検知出来た場合、

counted as correct if your algorithm correctly detects that the OOI is not in the frame.
これはOOIが正しく検知出来たものとして扱う。To be eligible for a prize, your submission needs to attain a minimum score of 700000 in System Testing.

T = total CPU processing time for all testing frames in seconds
検証フレームを処理したCPU時間の合計

AccuracyScore = 10000.0 * (50.0*(leftR[10]+rightR[10]) + 35.0*(leftR[20]+rightR[20]) + 15.0*(leftR[50]+rightR[50]))
正確なスコア

TimeMultiplier = 1.0 (if T <= 3.33), 1.3536 - 0.2939 * Ln(T) (if 3.33 < T <= 100.0), 0.0 (if T > 100.0)

Score = AccuracyScore * (1.0 + TimeMultiplier)

You can see these scores for example test cases when you make example test submissions. 
これらのスコアは、example提出時等に見ることが出来ます。

If your solution fails to produce a proper return value, your score for this test case will be 0.
もし返り値が正しくない場合は、スコアは0点として計算されます。

The overall score on a set of test cases is the arithmetic average of scores on single test cases from the set. 
全体のスコア計算として、各ケースの算術的平均値を使用します。

The match standings displays overall scores on provisional tests for all competitors who have made at least 1 full 
試合中のスコアは、完全な1ケースを完全にテストしたもので、

test submission. The winners are competitors with the highest overall scores on system tests.
この試合の勝者はシステムテスト全体のスコア計算の勝者となります。

Minimum Score Criteria
To be eligible for a prize, your submission needs to attain a minimum score of 700000 in System Testing.
あなたが賞金の対象となるためには、システムテストにおいて最低700,000点のスコアを獲得することが条件です。


Special rules and conditions
特別ルール、条件について

The allowed programming languages are C++, Java, C# and VB.
プログラミング言語にはC++, Java, C#およびVBを使用することが出来ます。

Be sure to see the official rules for details about open source library usage.
オープンソースのライブラリ使用については公式ルールを参照して下さい。

In order to receive the prize money, you will need to fully document your code and explain your algorithm. 
賞金を受け取るためには、あなたはコードを文章化、説明を行い、アルゴリズムを説明する義務があります。

If any parameters were obtained from the training data set, you will also need to provide the program 
任意のパラメータを生成している場合は、それを生成するためのプログラムの提出する必要があります。

used to generate these parameters. There is no restriction on the programming language used to generate 
                                    学習用にパラメータを生成する際には、言語の制限は存在しません。
these training parameters. Note that all this documentation should not be submitted anywhere during the 
                        全てのドキュメントはコンテスト期間中に他のところへの提出は認められません。
coding phase. Instead, if you win a prize, a TopCoder representative will contact you directly in order to collect this data.
                                              データを集めるためにTopCoderは直接あなたと連絡いたします。
You may use any external (outside of this competition) source of data to train your solution.
あなたの解答を集めるために