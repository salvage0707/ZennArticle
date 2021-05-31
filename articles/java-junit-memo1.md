---
title: "javaのJUnitの簡易メモ" 
emoji: "🏂"
type: "tech" 
topics: ["Java","JUnit","testing"]
published: true
---
# テストクラスのメソッド説明

前提クラス

	public class TestMethods {

	/**
	 * ブランクチェック
	 *
	 * @param value
	 *
	 * @return ブランク有無
	 * */
	static public boolean isBlank(String value) {
		if (value == null || value.length() == 0) {
			return true;
		}
		return false;
	}

	/**
	 * int値チェック
	 *
	 * @param value
	 * 			文字列
	 * @return int値判定有無
	 * */
	static public boolean isInt(String value) {
		if (isBlank(value)) {
			return false;
		}
		try {
			Integer.parseInt(value);
		} catch (NumberFormatException e) {
			return false;
		}
		return true;
	}
	}

` setUpBeforeClass `
 このテストクラスの最初のテストメソッドが呼び出される前に実行されるメソッド
` tearDownAfterClass `
 このテストクラスの最後のテストメソッドが呼び出された後に実行されるメソッド
` setUp `
 各テストメソッドが呼び出される前に実行されるメソッド
` tearDown `

## org.junit.Assert クラスが持つメソッド
`assertArrayEquaals`
期待値と実績値が等しい場合は正常終了。等しくない場合は失敗終了する
`asertEquals`
期待値と実績値が等しい場合は正常終了。等しくない場合は失敗終了
`assertFalse`
実績値が false の場合は正常終了。true の場合は失敗終了する
`assertNotNull`
実績値が null でない場合は正常終了。null の場合は失敗終了する
`assertNotSame`
期待値と実績値が同じ参照をしていない場合は正常終了。同じ参照をしている場合は失敗終了する
`assertNull`
実績値が null の場合は正常終了。null でない場合は失敗終了する
`assertSame`
期待値と実績値が同じ参照をしている場合は正常終了。同じ参照をしていない場合は失敗終了する
`assertTrue`
実績値が true の場合は正常終了。false の場合は失敗終了する
`fail`
このメソッドが実行されると失敗終了する
 各テストメソッドが呼び出さた後に実行されるメソッド
` testIsBlank `
 テスト対象クラスの isBlank メソッドに対するテストメソッド
` testIsInt `
 テスト対象クラスの isInt メソッドに対するテストメソッド

