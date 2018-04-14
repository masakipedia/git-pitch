---
title: "[Swift4] UIPageViewController with playground"
date: "2018-04-05T15:19:00+09:00"
tags: ["swift4", "xcode9", "UIPageViewController"]
bannerURL: "https://user-images.githubusercontent.com/32217053/38424615-9b605de6-39ec-11e8-9a46-4cc8466af85d.gif"
---


<h2>
    はじめに
</h2>
<p>
    UIPageViewController の仕組みを理解するのに苦労したので、
    Playgroud で試せるコードを実装しました。
</p>

<br>

<h2>
    コードはこちら
</h2>

    import UIKit
    import PlaygroundSupport


    class ContainerViewController: UIViewController {
        
        var pageViewController: UIPageViewController!
        
        // user setting
        let pageTotal: Int = 3
        
    }


    extension ContainerViewController {
        
        override func viewDidLoad() {
            super.viewDidLoad()
            
            pageViewController = UIPageViewController(transitionStyle: .scroll,
                                                    navigationOrientation: .horizontal,
                                                    options: nil)
            
            let firstViewController = generateViewControllerAtIndex(0)
            pageViewController.setViewControllers([firstViewController],
                                                direction: .forward,
                                                animated: true,
                                                completion: nil)
            
            pageViewController.dataSource = self
            
            self.addChildViewController(pageViewController)
            self.view.addSubview(pageViewController.view)
        }
        
    }


    // MARK: - DataSource

    extension ContainerViewController: UIPageViewControllerDataSource {
        
        func pageViewController(_ pageViewController: UIPageViewController, viewControllerAfter viewController: UIViewController) -> UIViewController? {
            let pageIndex = indexOfViewController(viewController as! RandomColorViewController)
            if pageIndex == pageTotal - 1 {
                return nil
            }
            return generateViewControllerAtIndex(pageIndex + 1)
        }
        
        func pageViewController(_ pageViewController: UIPageViewController, viewControllerBefore viewController: UIViewController) -> UIViewController? {
            let pageIndex = indexOfViewController(viewController as! RandomColorViewController)
            if pageIndex == 0 {
                return nil
            }
            return generateViewControllerAtIndex(pageIndex - 1)
        }
        
        func generateViewControllerAtIndex(_ index: Int) -> RandomColorViewController {
            let nextVC = RandomColorViewController()
            nextVC.index = index
            nextVC.view.backgroundColor = UIColor().getRandomColor()
            return nextVC
        }
        
        func indexOfViewController(_ viewController: RandomColorViewController) -> Int {
            return viewController.index
        }
        
    }


    class RandomColorViewController: UIViewController {
        
        var index: Int!
        
    }

    extension UIColor {
        
        func getRandomColor() -> UIColor{
            return UIColor(red: CGFloat(drand48()),
                        green: CGFloat(drand48()),
                        blue: CGFloat(drand48()),
                        alpha: 1.0)
            
        }
        
    }

    PlaygroundPage.current.liveView = ContainerViewController()


<br>

<h2>
    ハマったところ
</h2>
<p>
    ・UIpageViewControllerDataSource で返す UIViewController は、
    新しく生成したインスタンスである必要がある点です。
</p>
<p>
    ・DataSource でインスタンス生成を行わず、
    viewDidLoad などで全ての UIViewController を生成すると、真っ黒になりました。
</p>

<br>
<br>
<br>
