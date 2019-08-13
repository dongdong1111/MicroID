<template>
    <div class="tips-block">
        <section>
            <h4 class="tips-title">调拨实时进展</h4>
            <ol class="tips-content">
                <li>
                    <ul class="content-item">
                        <li>13:06</li>
                        <li>调拨指令已下发</li>
                    </ul>
                </li>
                <li>
                    <ul class="content-item">
                        <li>13:10</li>
                        <li>仓库门禁已进入战时出库状态</li>
                    </ul>
                </li>
                <li>
                    <ul class="content-item">
                        <li>13:10</li>
                        <li>仓库门禁已进入战时出库状态</li>
                    </ul>
                </li>
                <li>
                    <ul class="content-item">
                        <li>13:10</li>
                        <li>仓库门禁已进入战时出库状态</li>
                    </ul>
                </li>
                <li>
                    <ul class="content-item">
                        <li>13:11</li>
                        <li>仓库管理员已确认指令</li>
                    </ul>
                </li>
                <li>
                    <ul class="content-item">
                        <li>13:16</li>
                        <li>仓库器材已装车</li>
                    </ul>
                </li>
            </ol>
        </section>
    </div>
</template>

<script>
  export default {
    name: 'mapTips'
  }
</script>

<style lang="scss" scoped>
    .tips-block {
        position: absolute;
        top: 20px;
        bottom: 20px;
        right: 20px;
        border-radius: 4px;
        height: calc(100% - 40px);
        width: 300px;
        overflow: auto;
        background-color: #ffffff;
    }

    section {
        .tips-title {
            padding: 0 20px;
            color: #0a76a4;
        }

        .tips-content {
            padding-left: 20px;

            li {
                list-style: none;
            }

            .content-item {
                color: #999999;
                font-size: 14px;
                padding-left: 0;
                cursor: pointer;

                &:hover {
                    color: #646464;
                }

                &:after {
                    content: '';
                    display: block;
                    clear: both;
                }

                li {
                    float: left;
                    margin-bottom: 10px;

                    &:first-child {
                        padding-right: 10px;
                    }
                }
            }
        }
    }
</style>
