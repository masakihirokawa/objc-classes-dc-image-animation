DCImageAnimation
===============================

画像のタイマーアニメーションとコマ送りアニメーションを実行する「DCImageAnimation」クラスです。

アニメーションは主に以下のパラメータを渡して使用します。

1. アニメーションの種類（終了時のメソッド処理分岐用）
2. アニメーションのフレーム数
3. アニメーション画像ファイル名の接頭詞
4. アニメーションのリピート回数／ループの有無

デリケードメソッドを指定する事もでき、アニメーション終了後に任意のメソッドを呼ぶことができます。

##使用方法

###タイマーアニメーション

```objective-c
//タイマーアニメーション開始
- (void)startTimerAnimation
{
    //クラス初期化
    DCImageAnimation *timerAnimation = [[DCImageAnimation alloc] init];
    
    //アニメーション開始
    [timerAnimation startTimerAnimating:animationType //アニメーション名 (終了時の処理分岐用)
                                       :animationFrames //アニメーションの総フレーム数
                                       :animationPrefix //アニメーション画像の接頭詞
                                       :isLoopAnimation]; //アニメーションをループさせるか
    
    //ステージに追加
    [self.view addSubview:[timerAnimation timerAnimationImageView]];
}
```

###コマ送りアニメーション

```objective-c
//コマ送りアニメーション開始
- (void)startFrameAnimation
{
    //クラス初期化
    DCImageAnimation *frameAnimation = [[DCImageAnimation alloc] init];
    
    //アニメーション開始
    [frameAnimation startFrameAnimating:animationType //アニメーション名 (終了時の処理分岐用)
                                       :animationFrames //アニメーションの総フレーム数
                                       :animationPrefix //アニメーション画像の接頭詞
                                       :animationRepeatNum]; //アニメーションのリピート回数
    
    //ステージに追加
    [self.view addSubview:[frameAnimation frameAnimationImageView]];
}
```

##サンプルソースコード

###ViewController.h

```objective-c
#import <UIKit/UIKit.h>
#import "DCImageAnimation.h"

#define IMAGE_WIDTH  85
#define IMAGE_HEIGHT 120
#define ANIM_TYPE    2

@interface ViewController : UIViewController

@end
```

###ViewController.m

```objective-c
#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController

//アニメーションのタイプ
enum {
    TIMER_ANIMATION = 1, //タイマーアニメーション
    FRAME_ANIMATION = 2 //コマ送りアニメーション
};

- (void)viewDidLoad
{
    [super viewDidLoad];
    
    //アニメーション開始
    if (ANIM_TYPE == TIMER_ANIMATION) {
        //タイマーアニメーション
        [self startTimerAnimation];
    } else if (ANIM_TYPE == FRAME_ANIMATION) {
        //コマ送りアニメーション
        [self startFrameAnimation];
    }
}

//タイマーアニメーション開始
- (void)startTimerAnimation
{
    //クラス初期化
    DCImageAnimation *timerAnimation = [[DCImageAnimation alloc] init];
    
    //fps変更 (指定がない場合は 24fpsになります)
    [timerAnimation setFps:16.0f];
    
    //アニメーション画像のレクタングル指定 (指定がない場合は W320*H480pxになります)
    [timerAnimation setRectangle:CGRectMake(self.view.center.x - (IMAGE_WIDTH / 2),
                                            self.view.center.y - (IMAGE_HEIGHT / 2),
                                            IMAGE_WIDTH,
                                            IMAGE_HEIGHT)];
    
    //クラス側のデリゲートに渡す
    timerAnimation.dc_delegate = (id)self;
    
    //アニメーションのタイプ (アニメーション終了時の処理を分岐させる時に指定します)
    NSString *animationType = @"timer";
    
    //アニメーションの総フレーム数
    NSInteger animationFrames = 14;
    
    //アニメーションファイルの接頭詞
    NSString *animationPrefix = @"frame";
    
    //アニメーションをループさせるか
    BOOL isLoopAnimation = YES;
    
    //アニメーション開始
    [timerAnimation startTimerAnimating:animationType
                                       :animationFrames
                                       :animationPrefix
                                       :isLoopAnimation];
    
    //タグを指定 (省いても問題ありません)
    [timerAnimation timerAnimationImageView].tag = 0;
    
    //ステージに追加
    [self.view addSubview:[timerAnimation timerAnimationImageView]];
}

//コマ送りアニメーション開始
- (void)startFrameAnimation
{
    //クラス初期化
    DCImageAnimation *frameAnimation = [[DCImageAnimation alloc] init];
    
    //fps変更 (指定がない場合は 24fpsになります)
    [frameAnimation setFps:16.0f];
    
    //アニメーション画像のレクタングル指定 (指定がない場合は W320*H480pxになります)
    [frameAnimation setRectangle:CGRectMake(self.view.center.x - (IMAGE_WIDTH / 2),
                                            self.view.center.y - (IMAGE_HEIGHT / 2),
                                            IMAGE_WIDTH,
                                            IMAGE_HEIGHT)];
    
    //クラス側のデリゲートに渡す
    frameAnimation.dc_delegate = (id)self;
    
    //アニメーションのタイプ (アニメーション終了時の処理を分岐させる時に指定します)
    NSString *animationType = @"frame";
    
    //アニメーションの総フレーム数
    NSInteger animationFrames = 14;
    
    //アニメーションファイルの接頭詞
    NSString *animationPrefix = @"frame";
    
    //アニメーションのリピート回数 (0の場合は無限ループします)
    NSInteger animationRepeatNum = 0;
    
    //アニメーション開始
    [frameAnimation startFrameAnimating:animationType
                                       :animationFrames
                                       :animationPrefix
                                       :animationRepeatNum];
    
    //タグを指定 (省いても問題ありません)
    [frameAnimation frameAnimationImageView].tag = 0;
    
    //ステージに追加
    [self.view addSubview:[frameAnimation frameAnimationImageView]];
}

@end
```
