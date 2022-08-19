# Plan

|No.|command|param1|param2|param3|param4|Latitude|Longitude|Altitude(m)|param8|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|1|Tale off|0|0|0|0|0(hold current position)|0(hold current position)|1|Relative|
|2|GUIDED_ENABLE|1(enable)|0|0|0|0|0|0|Relative|
|3|LAND|0|0|0|0|0(hold current position)|0(hold current position)|0|Relative|

高度が一定以下になったときにdisarmをすることで着陸させる．

# Flow
1. autoモードに入れる（プロポ）
2. 現在位置を座標系の原点にする（GCS）
3. armする（GCS）
4. ミッションを開始する（GCS）
5. ARマーカーを認識した時点で自己位置推定を開始する（GCS）
6. Planを実行する
7. LANDモードで高度が一定以下になったときにDisarmする（GCS）

# class構成

```c++
class usar_auto_pilot

public:
/*
SDL_Renderから画像を受け取る関数
autoモードのときは画像処理のthreadを立てる．
*/
usar_auto_pilot::setImage(&cv::Mat)

/*
常に実行するタスク
@task1 telemtryを監視する
    自動モードに入ったかどうか
*/
usar_auto_pilot::mainTask()

/*
autoModeのときのみ実行するタスク
@task1 自己位置推定の結果を送信する
@task2 終了判定をする

if(isAutoMode == false) なにもしない
*/
usar_auto_pilot::autoModeTask()

private:
/*
ARマーカーを検知するタスク
*/
usar_auto_pilot::handleImage_ARTag()
/*
 白線を検知するタスク
*/
usar_auto_pilot::handleImage_Line()
std::shared_ptr<std::thread> thread_ARTag;
std::shared_ptr<std::thread> thread_Line;
bool isAutoMode;
```