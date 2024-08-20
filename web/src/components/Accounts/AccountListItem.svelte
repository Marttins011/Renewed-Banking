<script lang="ts">
    import { accounts, activeAccount, popupDetails, atm, translations } from "../../store/stores";
    import { formatMoney } from "../../utils/misc";
    export let account:any;

    function handleAccountClick(id: any) {
        activeAccount.update(() => id);
    };

    let isAtm: boolean;
    function handleButton(id:string, type:string) {
        let account = $accounts.find((accountItem: any) => id === accountItem.id);
        popupDetails.update(() => ({ actionType: type, account }));
    }

    atm.subscribe((usingAtm: boolean) => {
        isAtm = usingAtm;
    });
</script>

<section class="account" on:click={()=>handleAccountClick(account.id)}>
    <h4>
        {account.type}{$translations.account}/ {account.id}
    </h4>
    <h5>
        {account.type}{$translations.account}<br />
        <span>{account.name}</span>
    </h5>

    <div class="price">
        <strong>{formatMoney(account.amount)}</strong> <br />
        <span>{$translations.balance}</span>
    </div>

    <div class="btns-group">
        {#if !account.isFrozen}
            {#if !isAtm}
                <button class="btn btn-green" on:click={() => handleButton(account.id, "deposit")}>{$translations.deposit_but}</button>
            {/if}
            <button class="btn btn-red" on:click={() => handleButton(account.id, "withdraw")}>{$translations.withdraw_but}</button>
            <button class="btn btn-orange" on:click={() => handleButton(account.id, "transfer")}>{$translations.transfer_but}</button>
        {:else}
            {$translations.frozen}
        {/if}
    </div>
</section>

<style>
    .account {
        background-color: #242424;
        border: 1px solid #495057;
        padding: 1rem;
        border-radius: 0.25rem;
        cursor: pointer;
    }
    .account:not(:last-child) {
        margin-bottom: 1.5rem;
    }

    .btn-green {
        background-color: #40c057;
        color: #fff;
        border: none;
    }

    .btn-green:hover {
        background-color: #2f9e44;
        color: #fff;
        border: none;
    }

    .btn-red {
        background-color: #fa5252;
        color: #fff;
        border: none;
    }

    .btn-red:hover {
        background-color: #e03131;
        color: #fff;
        border: none;
    }

    .btn-orange {
        background-color: #fab005;
        color: #fff;
        border: none;
    }

    .btn-orange:hover {
        background-color: #f08c00;
        color: #fff;
        border: none;
    }

    h4 {
        font-size: 1.5rem;
        margin-bottom: 0.5rem;
    }
    h5 {
        font-size: 1.2rem;
    }
    h5 span {
        margin-top: 0.3rem;
    }

    .price {
        text-align: right;
        margin-bottom: 1rem;
    }
    .price strong {
        font-size: 1.6rem;
    }

    /* make first btn in btn-group take up the whole first row */
  .btns-group > :first-child {
    grid-column: 1 / -1;
  }
  .btns-group {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 0.5rem;
  }
</style>
