1. フェールオーバー クラスター マネージャーに戻ります。**[ロール]** を展開し、[可用性グループ] を強調表示します。**[リソース]** タブで、リスナー名を右クリックし、[プロパティ] をクリックします。

1. **[依存関係]** タブをクリックします。複数のリソースが一覧表示される場合は、IP アドレスに OR (AND ではなく) 依存関係があることを確認します。**[OK]** をクリックします。

1. リスナー名を右クリックし、**[オンラインにする]** をクリックします。

1. リスナーがオンラインになったら、**[リソース]** タブで可用性グループを右クリックし、**[プロパティ]** をクリックします。

	![可用性グループ リソースを構成する](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

1. リスナー名のリソース (IP アドレス リソース名ではなく) への依存関係を作成します。**[OK]** をクリックします。

	![リスナー名の依存関係を追加する](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

1. **SQL Server Management Studio** を起動し、プライマリ レプリカに接続します。

1. **[AlwaysOn 高可用性]**、**[可用性グループ]**、**<AvailabilityGroupName>**、**[可用性グループ リスナー]** の順に移動します。

3. フェールオーバー クラスター マネージャーで作成したリスナー名が表示されます。リスナー名を右クリックし、**[プロパティ]** をクリックします。

1. **[ポート]** ボックスで、以前に使用した $EndpointPort (このチュートリアルでは、既定値が 1433 でした) を使用し、可用性グループ リスナーのポート番号を指定して、**[OK]** をクリックします。

<!---HONumber=Oct15_HO3-->